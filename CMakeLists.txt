# CMakeLists
CMAKE_minimum_required(VERSION 3.10)

PROJECT(toy_project
  VERSION 0.0.1
  LANGUAGES CXX)

# code coverage - GoogleTest
option(CODE_COVERAGE "Enable coverage reporting" OFF)

if("${build}" MATCHES "release")
  message("define ... ${build}")
  SET (CMAKE_BUILD_TYPE Release)
else()
  message("define ... ${build}")
  SET (CMAKE_BUILD_TYPE Debug)  
endif() 

if (CODE_COVERAGE MATCHES ON)
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --coverage")	
  SET(BUILD_GMOCK ON)
endif()

SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib" CACHE PATH "Archive output dir.")
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib" CACHE PATH "Library output dir.")
SET(CMAKE_PDB_OUTPUT_DIRECTORY     "${CMAKE_BINARY_DIR}/bin" CACHE PATH "PDB (MSVC debug symbol)output dir.")
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin" CACHE PATH "Executable/dll output dir.")

set(INCLUDE_DIR ${PROJECT_SOURCE_DIR}/src)

set(PACKAGE_PATH ${CMAKE_SOURCE_DIR}/third_party)
message("PACKAGE_PATH: ${PACKAGE_PATH}")

# SPDLOG
set(SPDLOG_DIR ${PACKAGE_PATH}/spdlog)

if("${CMAKE_BUILD_TYPE}" MATCHES "Release")
   message("define SPDLOG_ACTIVE_LEVEL=SPDLOG_LEVEL_OFF")
  add_compile_options(-DSPDLOG_ACTIVE_LEVEL=SPDLOG_LEVEL_OFF
  -fPIC
  )
else()
  message("define SPDLOG_ACTIVE_LEVEL=SPDLOG_LEVEL_DEBUG")
  add_compile_options(-DSPDLOG_ACTIVE_LEVEL=SPDLOG_LEVEL_DEBUG
  -fPIC
  )
endif()


add_subdirectory(src)


if (CODE_COVERAGE MATCHES ON)
  enable_testing()
  add_subdirectory(test)
endif()