cmake_minimum_required(VERSION 3.2)

project(bionic)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0")

add_subdirectory(libc)
add_subdirectory(libm)
