cmake_minimum_required(VERSION 3.13)

include(../cmake/CPM.cmake)

CPMAddPackage("gh:izzyreal/cpp-project-template#main")

project(my-project-test)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "-fPIC")

if(DEFINED ENV{APPLE_M1})
    set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64" CACHE INTERNAL "")
endif()

if(APPLE)
  set(CMAKE_OSX_DEPLOYMENT_TARGET "10.11" CACHE INTERNAL "Minimum OS X deployment version")
endif()

set(_src_root_path "${CMAKE_CURRENT_SOURCE_DIR}/src")

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

add_executable(my-project-test ${_source_list})
include_directories(${_src_root_path} ${CMAKE_CURRENT_SOURCE_DIR}/../src)
target_link_libraries(my-project-test my-project)
