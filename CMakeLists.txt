cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_STANDARD 20)

project(bank)

add_library(account STATIC banking/Account.cpp banking/Account.h)
add_library(transaction STATIC banking/Transaction.cpp banking/Transaction.h)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/banking)

link_libraries(account transaction)