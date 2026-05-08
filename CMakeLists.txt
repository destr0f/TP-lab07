cmake_minimum_required(VERSION 3.10)

project(print)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TESTS "Build tests" OFF)

add_library(print STATIC
    sources/print.cpp
)

target_include_directories(print PUBLIC
    include
)

add_executable(example1
    examples/example1.cpp
)

add_executable(example2
    examples/example2.cpp
)

target_link_libraries(example1 print)
target_link_libraries(example2 print)

install(TARGETS print example1 example2
    RUNTIME DESTINATION bin
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY include/
    DESTINATION include
)

if(BUILD_TESTS)
    enable_testing()
    add_subdirectory(third-party/gtest)
    add_executable(check tests/test1.cpp)
    target_link_libraries(check print gtest_main)
    add_test(NAME check COMMAND check)
endif()
