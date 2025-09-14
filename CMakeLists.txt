cmake_minimum_required(VERSION 3.13)
project(AP)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(GTest REQUIRED)

include_directories(include/)

add_executable(main
        src/main.cpp
        src/unit_test.cpp
)
target_link_libraries(main
        GTest::gtest
        GTest::gtest_main
)