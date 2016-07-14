SLICOT Software Installation and Updating
-----------------------------------------

The next sections describe how to install/update the SLICOT Library,
and how to run the example programs.

SLICOT Library Installation/Updating

The essential source code and documentation for the SLICOT software is
distributed via an archive file, slicot.tar.gz or slicotPC.zip, for
Unix or Windows platforms, respectively.  After decompressing any of
these files, the SLICOT Library root directory, slicot, and several
filled-in subdirectories (benchmark_data, doc, examples, examples77,
src, and src_aux), will be created.

The distribution files mentioned above do not contain any object or
executable files.  They can be built using the information given
in this file.  Prebuilt libraries and MATLAB executables for
some often used platforms are available on the SLICOT Web site.
SLICOT routines make calls to subprograms from the state-of-the-art
packages LAPACK (Linear Algebra Package) and BLAS (Basic Linear
Algebra Subprograms).  Fortran source code and prebuilt, Fortran-based
LAPACK and BLAS libraries are freely downloadable from the netlib
(a system for the distribution of mathematical software through Internet).
However, for maximum efficiency it is recommended to use machine-specific,
optimized versions whenever possible.

Template make files are provided to help building the SLICOT Library
object file, and to link and run the available example programs calling
the SLICOT Library routines.  In order to use these make files on a
specific Unix or Windows platform, some changes might be needed in the
files make.inc and/or makefile stored in the SLICOT root directory, slicot.
Denote by <slicotroot> the path to the slicot directory, which can be,
e.g., c:\slicot, on Windows platforms.  (The last (sub)directory name
in <slicotroot> is slicot.)

The changes in make.inc might define the specific machine (platform)
identifier, the compiler, linker, and archiver flags, and the location
and names of the LAPACK and BLAS libraries, which the program files
should be linked to.  Details are given in the file make.inc.

Changes in makefile might be needed for using a Fortran 77 compiler,
since a Fortran 90/95 compiler is assumed by default to build the
executable example programs.  (The SLICOT routines themselves are
written in Fortran 77.)  Specifically, for a standard Fortran 77 compiler,
it is necessary to replace the string "examples" by "examples77" in the
file makefile.

After performing the necessary changes, as suggested in the comments of
the make files, the other needed SLICOT-related files can be generated
automatically with the command

make   # or nmake, for Windows platforms

issued from the directory slicot of <slicotroot>.

The first execution of (n)make will create the following files

- the SLICOT Library object files *.o (for Unix machines), or *.obj
  (for Windows machines), in the subdirectory src of slicot;
- the library file slicot.a or slicot.lib, respectively, in the
  directory slicot;
- the auxiliary library file lpkaux.a or lpkaux.lib, respectively,
  in the directory slicot;
- the example programs object and executable files, in the
  subdirectory examples(77);
- the files *.exa, with the results computed on the local machine,
  with the same name as for the files with data (*.dat) and
  reference results (*.res), also in the subdirectory examples(77).

The subsequent executions of (n)make will update the files if changes
have been performed.

The files *.exa, with the computed results may be compared with the
reference results.  Several types of differences could be noticed,
including possible sign changes for some elements, or even different
values in some columns and/or rows of the computed matrices.  For
instance, the matrices of similarity or equivalence transformations
could differ from a platform/compiler to another.  This does not
usually mean that the computed results are wrong.

More details for executing other tasks, e.g., cleaning the subdirectories
src and examples(77), are given in the files makefile included in
directory slicot and in the subdirectories src and examples(77).

The auxiliary library lpkaux contains few LAPACK and BLAS object files
for routines which slightly differ from the standard distribution.

The currently included files are dcabs1, dhgeqz, and dtgsy2.
Other files might be included, as specified in the Release.Notes or
Release.History files.

The file dcabs1 is a copy of the BLAS routine which might be missing
in some distributions.  To generate the object code for dcabs1, the
file makefile in the subdirectory src_aux should be modified accordingly
(see the included comments).

The file dhgeqz is the David Day's slightly modified version of the
corresponding LAPACK routine.  Without that modification, trivial
examples have been encountered on which the convergence of the
QZ algorithm was not achieved.  In addition, an error in the LAPACK
distribution has been removed.

Another error in the LAPACK distribution file dtgsy2 has been removed.

The first command-line invocation of (n)make from the SLICOT root
directory compiles the source files in subdirectory src_aux, and
automatically builds the object library file lpkaux.a or lpkaux.lib,
and store it in the directory slicot.

-----

Vasile Sima (vsima@ici.ro), September 26, 2009
