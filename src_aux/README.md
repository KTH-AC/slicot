SLICOT Library Subdirectory src_aux
-----------------------------------

SLICOT Library Subdirectory src_aux contains few auxiliary LAPACK and
BLAS source files which either slightly differ from the standard
distribution, or are explicitly needed for generating the SLICOT-based
MATLAB executable files (MEX-files) for a specific platform.

The currently included files are dcabs1, dhgeqz, dlagv2, and dtgsy2.
Other files might be included, as specified in the Release.Notes
or Release.History files.

The file dcabs1 is a copy of the BLAS routine which might be missing
in some distributions.  To generate the object code for dcabs1, the
file makefile in the subdirectory src_aux should be modified accordingly
(see the included comments).

The file dhgeqz is the David Day's slightly modified version of the
corresponding LAPACK routine.  Without that modification, trivial
examples have been encountered on which the convergence of the
QZ algorithm was not achieved.  In addition, an error in the LAPACK
distribution has been removed.

A variable used but not always initialized in the file dlagv2 has been set.
Another error in the LAPACK distribution file dtgsy2 has been removed.

More details are given in the file Installation.txt from the SLICOT
root directory.
