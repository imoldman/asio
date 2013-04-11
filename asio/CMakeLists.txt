########################################################################
#
# Global Config
#
# we use folders to manage target
set_property(GLOBAL PROPERTY USE_FOLDERS On)
########################################################################
#
# Project
#
project(asio CXX)
cmake_minimum_required(VERSION 2.6)
set (CMAKE_BACKWARDS_COMPATIBILITY 2.6)

# on windows, only support vc11
if (WIN32 AND NOT MSVC11)
  message(FATAL_ERROR "Cann't run in this VC platform toolset, "
                      "this project only support vc11")
endif()

# compiler and linker config
include("${CMAKE_CURRENT_SOURCE_DIR}/cmake/internal.cmake")
if (base_use_internal_build_config)
  set_default_compiler_linker_config()  # Defined in internal.cmake.
endif()

include_directories("D:/dummy-boost-include")
########################################################################
#
# Library
#
file(GLOB scripts RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    "cmake/internal.cmake")

source_group(scripts FILES ${scripts})

file(GLOB headers RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    "include/asio.hpp"
    "include/asio/*.hpp"
    "include/asio/*/*.hpp")

set(sources
    ${scripts}
    ${headers}
    "src/asio.cpp")

add_library(asio STATIC ${sources})
########################################################################
#
# Example
#
add_executable(chat_client "src/examples/chat/chat_client.cpp")
add_executable(chat_server "src/examples/chat/chat_server.cpp")
target_link_libraries(chat_client asio)
target_link_libraries(chat_server asio)
