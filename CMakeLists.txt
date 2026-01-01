cmake_minimum_required(VERSION 3.12)

project(cpp-argon-demo
    DESCRIPTION "CPP-ARGON: Collection of the library demo projects"
    LANGUAGES CXX
)

# Build options
set(OUT_DIR ${CMAKE_CURRENT_BINARY_DIR})
set(ARGON_TAG "master" CACHE STRING "The branch or tag name used to fetch the CPP-ARGON library")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(DEFAULT_CXX_FLAGS "-Wall -Wextra -Wcast-align -Wconversion -Wunreachable-code -Wuninitialized -pedantic -g -O2")
if (NOT CMAKE_CXX_FLAGS)
    set(CMAKE_CXX_FLAGS "${DEFAULT_CXX_FLAGS}" CACHE STRING "Default C++ compiler flags" FORCE)
endif()

message(STATUS "Build options:")
message(STATUS "[OUT_DIR = ${OUT_DIR}]")
message(STATUS "[ARGON_TAG = ${ARGON_TAG}]")
message(STATUS "[CMAKE_CXX_FLAGS = ${CMAKE_CXX_FLAGS}]")

# Fetch CPP-ARGON library
include(FetchContent)
FetchContent_Declare(
    cpp-argon
    GIT_REPOSITORY https://github.com/SpectraL519/cpp-argon.git
    GIT_TAG ${ARGON_TAG}
)
FetchContent_MakeAvailable(cpp-argon)

macro(make_demo PROJECT_NAME TARGET_NAME)
    cmake_minimum_required(VERSION 3.12)

    project(cpp-argon-demo-${PROJECT_NAME}
        DESCRIPTION "CPP-ARGON library demo project: ${PROJECT_NAME}"
        LANGUAGES CXX
    )

    # Define the executable for the project
    add_executable(${TARGET_NAME} main.cpp)
    target_include_directories(${TARGET_NAME} PRIVATE include)
    set_target_properties(${TARGET_NAME} PROPERTIES OUTPUT_NAME "run")

    # Link the cpp-argon library
    target_link_libraries(${TARGET_NAME} PRIVATE cpp-argon)
endmacro()


# Build projects
add_subdirectory(argon_git)
add_subdirectory(file_merger)
add_subdirectory(logging_mode)
add_subdirectory(message_logger)
add_subdirectory(numbers_converter)
add_subdirectory(power_calculator)
add_subdirectory(verbosity)
