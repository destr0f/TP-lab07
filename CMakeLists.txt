cmake_minimum_required(VERSION 3.10)

include("cmake/HunterGate.cmake")

HunterGate(
  URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"
  SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"
  LOCAL
)

project(print)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TESTS "Build tests" OFF)

add_library(print STATIC sources/print.cpp)
target_include_directories(print PUBLIC include)

add_executable(example1 examples/example1.cpp)
add_executable(example2 examples/example2.cpp)

target_link_libraries(example1 print)
target_link_libraries(example2 print)

if(BUILD_TESTS)
  hunter_add_package(GTest)
  find_package(GTest CONFIG REQUIRED)

  enable_testing()

  add_executable(check tests/test1.cpp)
  target_link_libraries(check print GTest::gtest_main)
  add_test(NAME check COMMAND check)
endif()

add_executable(demo demo/main.cpp)
target_link_libraries(demo print)

hunter_add_package(timp_message)
find_package(timp_message CONFIG REQUIRED)

add_executable(custom_package_demo custom_package_demo/main.cpp)
target_link_libraries(custom_package_demo timp_message::timp_message)
