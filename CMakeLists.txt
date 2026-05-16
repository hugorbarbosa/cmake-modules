#
# Copyright (C) 2026 Hugo Barbosa.
#

# ----------------------------------------------------------------------------
# Project setup
# ----------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.28)

project(
    cmake_modules
    VERSION 1.0.0
    LANGUAGES CXX
    DESCRIPTION "Collection of reusable CMake modules."
)

set(CMAKEMOD_PROJECT_NAME ${PROJECT_NAME})
set(CMAKEMOD_PROJECT_VERSION ${PROJECT_VERSION})

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/modules")

# ----------------------------------------------------------------------------
# Pre-requirements
# ----------------------------------------------------------------------------
include(SourceBuilds)
validate_build_directory()

# ----------------------------------------------------------------------------
# Git information
# ----------------------------------------------------------------------------
include(GitInfo)
get_git_info("GIT_INFO")

# ----------------------------------------------------------------------------
# Configuration options
# ----------------------------------------------------------------------------
option(CMAKEMOD_ENABLE_CMAKE_FORMAT "Enable CMake code formatting with cmake-format" OFF)
option(CMAKEMOD_ENABLE_CMAKE_LINT "Enable CMake lint with cmake-lint" OFF)

# ----------------------------------------------------------------------------
# CMake options
# ----------------------------------------------------------------------------
if(PROJECT_IS_TOP_LEVEL)
    if(NOT CMAKE_BUILD_TYPE)
        set(default_build_type Debug)
        message(STATUS "No build type specified, setting it to '${default_build_type}'")
        set(CMAKE_BUILD_TYPE ${default_build_type})
    endif()

    # Generates a JSON file containing the exact compiler commands. Useful for clang-based tools.
    set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
endif()

# ----------------------------------------------------------------------------
# Quality tools
# ----------------------------------------------------------------------------

if(CMAKEMOD_ENABLE_CMAKE_FORMAT)
    include(CmakeFormat)
    add_cmake_format(
        DIRECTORIES
        "${CMAKE_CURRENT_SOURCE_DIR}/modules"
        EXTRA_FILES
        "${CMAKE_CURRENT_SOURCE_DIR}/CMakeLists.txt"
        LOG_FILE
        "${CMAKE_BINARY_DIR}/cmake_format_report.log"
        CONFIG_FILE
        "${CMAKE_CURRENT_SOURCE_DIR}/.cmake-format.py"
    )
endif()

if(CMAKEMOD_ENABLE_CMAKE_LINT)
    include(CmakeLint)
    add_cmake_lint(
        DIRECTORIES
        "${CMAKE_CURRENT_SOURCE_DIR}/modules"
        EXTRA_FILES
        "${CMAKE_CURRENT_SOURCE_DIR}/CMakeLists.txt"
        LOG_FILE
        "${CMAKE_BINARY_DIR}/cmake_lint_report.log"
        CONFIG_FILE
        "${CMAKE_CURRENT_SOURCE_DIR}/.cmake-format.py"
    )
endif()

# ----------------------------------------------------------------------------
# Configuration report
# ----------------------------------------------------------------------------
message(STATUS)
message(STATUS "${CMAKEMOD_PROJECT_NAME} ${CMAKEMOD_PROJECT_VERSION} configuration:")
message(STATUS "- CMAKE_BUILD_TYPE = ${CMAKE_BUILD_TYPE}")
message(STATUS "- CMAKE_CXX_COMPILER_ID = ${CMAKE_CXX_COMPILER_ID}")
message(STATUS "- CMAKE_CXX_COMPILER_VERSION = ${CMAKE_CXX_COMPILER_VERSION}")
message(STATUS "- CMAKE_CXX_STANDARD = ${CMAKE_CXX_STANDARD}")
message(STATUS "- CMAKE_EXPORT_COMPILE_COMMANDS = ${CMAKE_EXPORT_COMPILE_COMMANDS}")
message(STATUS "- PROJECT_IS_TOP_LEVEL = ${PROJECT_IS_TOP_LEVEL}")
message(STATUS "- CMAKEMOD_ENABLE_CMAKE_FORMAT = ${CMAKEMOD_ENABLE_CMAKE_FORMAT}")
message(STATUS "- CMAKEMOD_ENABLE_CMAKE_LINT = ${CMAKEMOD_ENABLE_CMAKE_LINT}")
message(STATUS)
