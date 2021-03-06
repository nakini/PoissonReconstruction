cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

SET_PROPERTY(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)

IF(WIN32 OR win64)
   SET(CMAKE_FIND_LIBRARY_SUFFIXES .lib .dll)
ELSE()
   SET(CMAKE_FIND_LIBRARY_SUFFIXES .a)
ENDIF()


find_package(PCL 1.7 REQUIRED)

project(PoissonReconstruction)

add_subdirectory(src)

include_directories(${PCL_INCLUDE_DIRS})
include_directories(${PoissonReconstruction_SOURCE_DIR}/include)

link_directories(${PCL_LIBRARY_DIRS})

add_definitions(${PCL_DEFINITIONS})

file(GLOB_RECURSE PoissonReconstruction_HEADERS include/*.h)
file(GLOB_RECURSE PoissonReconstruction_SOURCES src/*.cpp)
set (PoissonReconstruction_INCLUDE_DIRS "")
foreach (_headerFile ${PoissonReconstruction_HEADERS})
    get_filename_component(_dir ${_headerFile} PATH)
    list (APPEND PoissonReconstruction_INCLUDE_DIRS ${_dir})
endforeach()
list(REMOVE_DUPLICATES PoissonReconstruction_INCLUDE_DIRS)

include_directories(${PoissonReconstruction_INCLUDE_DIRS})

add_executable (PoissonReconstruction ${PoissonReconstruction_SOURCES} ${PoissonReconstruction_HEADERS})
target_link_libraries (PoissonReconstruction ${PCL_LIBRARIES})