cmake_minimum_required(VERSION 3.13)

# include(../cmake/CPM.cmake)

# Add dependencies
# CPMAddPackage("gh:github_user/repo#tag")

set(MY_PROJECT_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}" CACHE STRING "Setting MY_PROJECT_SOURCE_DIR")
set(MY_PROJECT_BINARY_DIR "${CMAKE_CURRENT_BINARY_DIR}" CACHE STRING "Setting MY_PROJECT_BINARY_DIR")

project(my-project)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "-fPIC")

if(DEFINED ENV{APPLE_M1})
    set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64" CACHE INTERNAL "")
endif()

if(APPLE)
  set(CMAKE_OSX_DEPLOYMENT_TARGET "10.11" CACHE INTERNAL "Minimum OS X deployment version")
endif()

set(_src_root_path "${CMAKE_CURRENT_SOURCE_DIR}/src")

include_directories(${_src_root_path} ${rapidjson_SOURCE_DIR})

file(
    GLOB_RECURSE _source_list
    LIST_DIRECTORIES false
    RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    "${_src_root_path}/*.c*"
    "${_src_root_path}/*.h*"
)

foreach(_source IN ITEMS ${_source_list})
    get_filename_component(_source_path "${_source}" PATH)
    string(REPLACE "/" "\\" _source_path_msvc "${_source_path}")
    source_group("${_source_path_msvc}" FILES "${_source}")
endforeach()

add_library(my-project ${_source_list})
