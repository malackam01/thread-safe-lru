cmake_minimum_required(VERSION 3.5)
project(thread-safe-lru)

add_executable(benchmark benchmark.cpp)

add_library(thread-safe-lru INTERFACE)
target_include_directories(thread-safe-lru INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

include(FindPkgConfig)
pkg_search_module(TBB REQUIRED tbb)
target_link_libraries(benchmark ${TBB_LIBRARIES})
include_directories(${TBB_INCLUDE_DIRS})

# Boost is needed for the benchmark only
if(POLICY CMP0167)
    cmake_policy(SET CMP0167 NEW)
endif()
find_package(Boost REQUIRED COMPONENTS locale)
target_link_libraries(benchmark ${Boost_LIBRARIES})
include_directories(${Boost_INCLUDE_DIRS})

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
set(CMAKE_CXX_FLAGS "-std=c++11 -O3")
