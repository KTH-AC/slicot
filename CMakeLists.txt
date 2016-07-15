# cmake version required
cmake_minimum_required (VERSION 3.0)

# project related information
project (Slicot VERSION 5.0 LANGUAGES Fortran)

# Fortran compiler checker is needed
include (CheckFortranCompilerFlag)

# Try checking if the user supplied libraries work
include (CheckFortranFunctionExists)

set (BlasUsable FALSE)
set (LapackUsable FALSE)

if (DEFINED BlasLibrary AND EXISTS ${BlasLibrary})
  message (STATUS "BlasLibrary: ${BlasLibrary}")
  message (STATUS "Looking for Fortran sgemm")
  set (CMAKE_REQUIRED_LIBRARIES ${BlasLibrary})
  CHECK_FORTRAN_FUNCTION_EXISTS (sgemm blascheck)

  if (blascheck)
    message (STATUS "Looking for Fortran sgemm - found")
    set (BlasUsable TRUE)
    set (ExtLibs ${CMAKE_REQUIRED_LIBRARIES})
    message (STATUS "BLAS library used: ${BlasLibrary}")

    # Check for LAPACK functionality (maybe OpenBLAS?)
    message (STATUS "Looking for Fortran cheev")
    CHECK_FORTRAN_FUNCTION_EXISTS (cheev lapackcheck1)
    if (lapackcheck1)
      message (STATUS "Looking for Fortran cheev - found")
      message (STATUS "LAPACK library used: ${BlasLibrary}")
      set (LapackUsable TRUE)
    endif (lapackcheck1)

    set (CMAKE_REQUIRED_LIBRARIES)
  endif (blascheck)
endif ()

if ((NOT LapackUsable) AND (DEFINED LapackLibrary AND EXISTS ${LapackLibrary}))
  message (STATUS "LapackLibrary: ${LapackLibrary}")
  message (STATUS "Looking for Fortran cheev")
  set (CMAKE_REQUIRED_LIBRARIES ${LapackLibrary})
  CHECK_FORTRAN_FUNCTION_EXISTS (cheev lapackcheck2)

  if (lapackcheck2)
    message (STATUS "Looking for Fortran cheev - found")
    set (LapackUsable TRUE)
    set (ExtLibs ${ExtLibs} ${CMAKE_REQUIRED_LIBRARIES})
    message (STATUS "LAPACK library used: ${LapackLibrary}")
    set (CMAKE_REQUIRED_LIBRARIES)
  endif (lapackcheck2)
endif ()

if ((NOT BlasUsable) OR (NOT LapackUsable))
  include (FindLAPACK)

  if (NOT LAPACK_FOUND)
    message (SEND_ERROR "Slicot depends on LAPACK: NONE FOUND")
  else()
    if (LAPACK95_FOUND)
      set (ExtLibs ${ExtLibs} ${LAPACK95_LIBRARIES})
      message (STATUS "LAPACK95 found: ${LAPACK95_LIBRARIES}")
    else ()
      set (ExtLibs ${ExtLibs} ${LAPACK_LIBRARIES})
      message (STATUS "LAPACK found: ${LAPACK_LIBRARIES}")
    endif ()
  endif ()
endif ()

message(STATUS "ExtLibs: ${ExtLibs}")

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
