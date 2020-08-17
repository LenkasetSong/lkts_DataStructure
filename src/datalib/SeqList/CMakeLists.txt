include(CMakeToolsHelpers OPTIONAL)

cmake_minimum_required(VERSION 2.8)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

# 包含公共库
include_directories(${CMAKE_SOURCE_DIR}/common)

aux_source_directory(. DIR_LIB_SRCS)

# 生成链接库
add_library(SeqList ${DIR_LIB_SRCS})