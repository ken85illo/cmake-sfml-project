cmake_minimum_required(VERSION 3.28)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPERIMENTAL_CXX_MODULES OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

project(CMakeSFMLProject LANGUAGES CXX)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")

set(INCLUDE_DIR "./include" "./src")

include(FetchContent)

FetchContent_Declare(
    SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG 3.0.1
    GIT_SHALLOW ON
    EXCLUDE_FROM_ALL 
    SYSTEM
)

FetchContent_MakeAvailable(SFML)

file(
    GLOB_RECURSE SOURCE_LIST
    RELATIVE ${CMAKE_SOURCE_DIR}
    "src/*.cpp" 
)

add_executable(${PROJECT_NAME} ${SOURCE_LIST})
target_precompile_headers(${PROJECT_NAME} PRIVATE "$<$<COMPILE_LANGUAGE:CXX>:pch.hpp>")
target_include_directories(${PROJECT_NAME} PRIVATE ${INCLUDE_DIR})
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)
target_link_libraries(${PROJECT_NAME} PRIVATE SFML::Graphics)
