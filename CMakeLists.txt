cmake_minimum_required(VERSION 2.8)

project(fortran-machine C Fortran)

set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

find_package (SQLite3 REQUIRED)
# SQLITE3_INCLUDE_DIRS	- where to find sqlite3.h, etc.
# SQLITE3_LIBRARIES	- List of libraries when using sqlite.
# SQLITE3_FOUND	- True if sqlite found.
find_package (FCGI REQUIRED)
find_package(Threads REQUIRED)

include_directories(${SQLITE3_INCLUDE_DIRS})

add_executable(fortran_fcgi 
  fortran_fcgi.f90
  marsupial.f90
  jade.f90
  string_helpers.f90
  # Ugly, but avoids descending into the directory
  ${CMAKE_SOURCE_DIR}/flibs-0.9/flibs/src/sqlite/fsqlite.f90
  ${CMAKE_SOURCE_DIR}/flibs-0.9/flibs/src/cgi/cgi_protocol.f90
  ${CMAKE_SOURCE_DIR}/flibs-0.9/flibs/src/cgi/fcgi_protocol.f90
  ${CMAKE_SOURCE_DIR}/flibs-0.9/flibs/src/sqlite/csqlite.c
) 

set_property(SOURCE ${CMAKE_SOURCE_DIR}/flibs-0.9/flibs/src/sqlite/csqlite.c
             PROPERTY COMPILE_DEFINITIONS "-DLOWERCASE")

target_link_libraries(fortran_fcgi
  ${CMAKE_DL_LIBS}
  ${CMAKE_THREAD_LIBS_INIT}
  ${SQLITE3_LIBRARIES}
  ${FCGI_LIBRARIES}
)

