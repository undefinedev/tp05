cmake_minimum_required(VERSION 3.18)

set(CMAKE_CXX_STANDARD 20)

option(BUILD_TESTS "Build tests" OFF)
option(COVERAGE "Check coverage" OFF)

project(bank)

add_library(account STATIC banking/Account.cpp banking/Account.h)
add_library(transaction STATIC banking/Transaction.cpp banking/Transaction.h)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/banking)

find_package(GTest CONFIG REQUIRED)

if(BUILD_TESTS)
    add_executable(bank_test ${CMAKE_CURRENT_SOURCE_DIR}/tests/test.cpp)
    target_link_libraries(bank_test account transaction GTest::gtest_main GTest::gmock_main)
    enable_testing()
    include(GoogleTest)
endif()

if (COVERAGE)
    target_compile_options(check PRIVATE --coverage)
    target_link_libraries(check --coverage)
endif()