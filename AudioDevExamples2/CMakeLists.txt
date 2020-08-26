cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

set( CMAKE_VERBOSE_MAKEFILE on )

project(AudioDevExamples VERSION 1.0.0 LANGUAGES CXX)

if( NOT CMAKE_BUILD_TYPE )
  set( CMAKE_BUILD_TYPE Release CACHE STRING
       "Choose the type of build, options are: None Debug Release RelWithDebInfo
MinSizeRel."
       FORCE )
endif()

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake_find")
include (cmake_find/FindJUCE-GIT.cmake)

set(PYTORCH_VERSION "1.6.0")

find_package(Torch ${PYTORCH_VERSION} EXACT QUIET PATHS "${CMAKE_CURRENT_BINARY_DIR}/_deps/libtorch")
if(NOT Torch_FOUND)
    unset(Torch_FOUND)
    include(download_libtorch)    
endif()



#adding project folders:
add_subdirectory(Standalone)
# add_subdirectory(Plugins)

