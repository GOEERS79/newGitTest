cmake_minimum_required(VERSION 3.10)

SET( CMAKE_CXX_COMPILER "/MinGW64/bin/g++" )
set(CMAKE_CXX_FLAGS "-std=c++14")
add_definitions(-DTESTCODE)

project (MyProject)
  

set(sources src/move.cpp)

configure_file (
  "${PROJECT_SOURCE_DIR}/configuration.h.in"
  "${PROJECT_BINARY_DIR}/configuration.h"
)

# does this system provide the log and exp functions?

include (CheckFunctionExists)
check_function_exists (log HAVE_LOG)
check_function_exists (exp HAVE_EXP)
 
# Set the output folder where your program will be created
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
#set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR})

#set(CMAKE_LIB_DIR ${CMAKE_SOURCE_DIR}/lib)
#set(LIBRARY_OUTPUT_PATH ${CMAKE_LIB_DIR})

# The following folder will be included
include_directories("${PROJECT_SOURCE_DIR}")

# add the binary tree to the search path for include files
# so that we will find configuration.h
include_directories("${PROJECT_BINARY_DIR}")
#include_directories("${PROJECT_BINARY_DIR}/libs")

# should we use our own math functions?
#option (USE_MYMATH "Use tutorial provided math implementation" ON)
option (USE_LIBS "Use tutorial provided math implementation" OFF)

if (USE_LIBS)
  include_directories ("${PROJECT_SOURCE_DIR}/libs")
  add_subdirectory (libs)
  set (EXTRA_LIBS ${EXTRA_LIBS} JoeLibs)
endif (USE_LIBS)


add_executable(move ${sources})
target_link_libraries (move ${EXTRA_LIBS})

# add the install targets
install (TARGETS move DESTINATION bin)
#install (FILES "${PROJECT_BINARY_DIR}/utils.hpp" DESTINATION include)

