cmake_minimum_required(VERSION 3.5)

project(linenoise)

add_library(linenoise INTERFACE)

target_include_directories(linenoise INTERFACE .)
