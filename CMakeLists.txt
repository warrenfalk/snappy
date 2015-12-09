project(snappy)
cmake_minimum_required(VERSION 3.0)

set (PACKAGE_NAME      "snappy")
set (PACKAGE_VERSION   "1.1.3")
set (SNAPPY_MAJOR "1")
set (SNAPPY_MINOR "1")
set (SNAPPY_PATCHLEVEL "3")

set (SNAPPY_SRCS
  "snappy.cc"
  "snappy-sinksource.cc"
  "snappy-stubs-internal.cc"
  "snappy-c.cc")

include(${CMAKE_ROOT}/Modules/CheckIncludeFile.cmake)

macro(check_include_file_bool include variable)
  check_include_file(${include}, ${variable}_found)
  if ("${${variable}_found}")
    set(${variable} 1 CACHE INTERNAL "Have include ${include}")
  else ()
    set(${variable} 0 CACHE INTERNAL "Have include ${include}")
  endif ()
endmacro()

check_include_file_bool("stdint.h" ac_cv_have_stdint_h)
check_include_file_bool("stddef.h" ac_cv_have_stddef_h)
check_include_file_bool("sys/uio.h" ac_cv_have_sys_uio_h)

configure_file("snappy-stubs-public.h.in"
	"${PROJECT_BINARY_DIR}/snappy-stubs-public.h")

configure_file("snappy.h"
  "${PROJECT_BINARY_DIR}/snappy.h")

include_directories(${PROJECT_BINARY_DIR})

#configure_sources(${GFLAGS_SRCS})

add_library(libsnappy STATIC
  ${SNAPPY_SRCS}
  )
