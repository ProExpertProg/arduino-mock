cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_FLAGS "-std=c++14")
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/build")

project(arduino-mock)

if (NOT TARGET gtest)
    add_subdirectory(lib/googletest)
endif ()
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR} ${gmock_SOURCE_DIR}/include ${gmock_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

add_library(arduino-mock STATIC
        src/Arduino.cc src/EEPROM.cc src/Serial.cc src/serialHelper.cc src/Spark.cc src/SPI.cc
        src/WiFi.cc src/RedBot.cc src/Wire.cc)

set_target_properties(arduino-mock PROPERTIES ENABLE_EXPORTS true)

target_link_libraries(arduino-mock gtest gtest_main gmock)

option(test "Build all tests." OFF)

if (test)
    enable_testing()
    message("building tests for Arduino Mock")

    add_executable(arduino-mock-test
            test/test_all.cc test/Arduino_unittest.cc test/EEPROM_unittest.cc test/Serial_unittest.cc
            test/serialHelper_unittest.cc test/Spark_unittest.cc test/SPI_unittest.cc test/Stream_unittest.cc
            test/WiFi_unittest.cc test/Wire_unittest.cc)
    target_link_libraries(arduino-mock-test gtest gtest_main gmock)
    target_link_libraries(arduino-mock-test arduino-mock)
    add_test(arduino-mock arduino-mock-test)
endif ()
