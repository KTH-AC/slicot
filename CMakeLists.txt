# cmake version required
cmake_minimum_required (VERSION 3.0)

# project related information
project (Slicot
        VERSION 5.0.20101122
        LANGUAGES Fortran)

# Fortran compiler checker is needed
include (CheckFortranCompilerFlag)

# BLAS and LAPACK libraries are needed
# include (FindBLAS) : not needed as FindLAPACK handles both
include (FindLAPACK)

# if (NOT BLAS_FOUND)
#   message(SEND_ERROR "Slicot depends on BLAS")
# elseif (NOT LAPACK_FOUND)
if (NOT LAPACK_FOUND)
  message (SEND_ERROR "Slicot depends on LAPACK")
else()
  # if (BLAS95_FOUND)
  #   set (ExtLibs ${ExtLibs} ${BLAS95_LIBRARIES})
  #   message (STATUS "BLAS95 found: ${BLAS95_LIBRARIES}")
  # else ()
  #   set (ExtLibs ${ExtLibs} ${BLAS_LIBRARIES})
  #   message (STATUS "BLAS found: ${BLAS_LIBRARIES}")
  # endif ()
  if (LAPACK95_FOUND)
    set (ExtLibs ${ExtLibs} ${LAPACK95_LIBRARIES})
    message (STATUS "LAPACK95 found: ${LAPACK95_LIBRARIES}")
  else ()
    set (ExtLibs ${ExtLibs} ${LAPACK_LIBRARIES})
    message (STATUS "LAPACK found: ${LAPACK_LIBRARIES}")
  endif ()
endif ()

# make sure that the default build is a release
if (NOT CMAKE_BUILD_TYPE)
  set (CMAKE_BUILD_TYPE Release CACHE STRING
      "Type of build: Debug [Release]"
      FORCE)
endif (NOT CMAKE_BUILD_TYPE)

# default installation folder
get_filename_component (default_installdir ".." ABSOLUTE)
set (CMAKE_INSTALL_PREFIX ${default_installdir} CACHE STRING
      "Installation directory: [${default_installdir}]"
      FORCE)

CHECK_Fortran_COMPILER_FLAG ("-funroll-all-loops -fno-f2c -O3" ReleaseValid)
CHECK_Fortran_COMPILER_FLAG ("-fno-f2c -O0 -g" DebugValid)

if (ReleaseValid)
  set (CMAKE_Fortran_FLAGS_RELEASE "-funroll-all-loops -fno-f2c -O3")
else ()
  set (CMAKE_Fortran_FLAGS_RELEASE "-O3")
endif ()

if (DebugValid)
  set (CMAKE_Fortran_FLAGS_DEBUG "-fno-f2c -O0 -g")
else ()
  set (CMAKE_Fortran_FLAGS_DEBUG "-O0 -g")
endif ()

add_subdirectory (src)
add_subdirectory (src_aux)
