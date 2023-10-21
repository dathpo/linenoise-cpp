cmake_minimum_required(VERSION 3.5)

add_executable(example example.cpp)

target_compile_features(example PRIVATE cxx_std_14)

target_include_directories(example PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})