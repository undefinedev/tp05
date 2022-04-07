cmake_minimum_required(VERSION 3.18)

set(CMAKE_CXX_STANDARD 20)

option(BUILD_TESTS "Build tests" OFF)
option(COVERALLS "Check coverage" OFF)

project(bank)

add_library(account STATIC banking/Account.cpp banking/Account.h)
add_library(transaction STATIC banking/Transaction.cpp banking/Transaction.h)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/banking)

include(FetchContent)
FetchContent_Declare(
        googletest
        URL https://github.com/google/googletest/archive/609281088cfefc76f9d0ce82e1ff6c30cc3591e5.zip
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

if(BUILD_TESTS)
    include(GoogleTest)
    enable_testing()
    add_executable(bank_test ${CMAKE_CURRENT_SOURCE_DIR}/tests/test.cpp)
    target_link_libraries(bank_test account transaction gtest_main gmock_main)
    gtest_discover_tests(bank_test)
endif()

if (COVERALLS)
    add_executable(bank_test ${CMAKE_CURRENT_SOURCE_DIR}/tests/test.cpp)
    target_link_libraries(bank_test account transaction gtest_main gmock_main)
    target_compile_options(bank_test PRIVATE --coverage)
    target_link_libraries(bank_test --coverage)
endif()