cmake_minimum_required(VERSION 3.1)
project(FTPL)

set(CMAKE_CXX_STANDARD 14)#Supported values are 98, 11 and 14.
set(CMAKE_CXX_FLAGS "-Wall")
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_BUILD_TYPE Debug)
set(BUILD_SHARED_LIBS True)

#Threads lib
find_package (Threads)

include_directories(${CMAKE_SOURCE_DIR}/..)

SET(EXAMPLE_SOURCES example.cpp)
add_executable(example ${EXAMPLE_SOURCES})
set_target_properties(
    example 
    PROPERTIES 
        OUTPUT_NAME "example"
        SUFFIX ""
)
target_link_libraries(example ${CMAKE_THREAD_LIBS_INIT})
