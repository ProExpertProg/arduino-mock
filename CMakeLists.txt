cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_FLAGS "-std=c++98")
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/build")

project(arduino-mock)

file(GLOB HEADER_FILES *.h)

if(NOT TARGET gtest)
    add_subdirectory(lib/googletest)
endif()

add_library(arduino-mock STATIC "src/ArduinoMockAll.cc" ${HEADER_FILES})

include_directories(arduino-mock PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR} ${gmock_SOURCE_DIR}/include ${gmock_SOURCE_DIR})

set_target_properties(arduino-mock PROPERTIES ENABLE_EXPORTS true)

target_link_libraries(arduino-mock gtest gtest_main gmock)


option(test "Build all tests." OFF)

if (test)
    enable_testing()
    add_subdirectory(test)
endif ()
