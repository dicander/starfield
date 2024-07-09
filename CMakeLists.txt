cmake_minimum_required(VERSION 3.10)

# Set the project name
project(starfield)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Specify the output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Add source files
file(GLOB_RECURSE SOURCES "src/*.cpp")

# Include directories
include_directories(include)

# Find OpenGL
cmake_policy(SET CMP0072 NEW)
find_package(OpenGL REQUIRED)
if (NOT OPENGL_FOUND)
    message(ERROR "OpenGL not found!")
endif()

# Find GLEW
find_package(GLEW REQUIRED)
if (NOT GLEW_FOUND)
    message(ERROR "GLEW not found!")
endif()

# Find GLFW
find_package(glfw3 REQUIRED)
if (NOT glfw3_FOUND)
    message(ERROR "GLFW not found!")
endif()

# Add GLM manually
set(GLM_INCLUDE_DIR "/usr/include/glm")
if (NOT EXISTS ${GLM_INCLUDE_DIR})
    message(FATAL_ERROR "GLM include directory not found at ${GLM_INCLUDE_DIR}")
endif()
include_directories(${GLM_INCLUDE_DIR})

# Add executable
add_executable(${PROJECT_NAME} ${SOURCES})

# Link libraries
target_link_libraries(${PROJECT_NAME} OpenGL::GL GLEW::GLEW glfw)

# Set include directories
target_include_directories(${PROJECT_NAME} PRIVATE ${GLM_INCLUDE_DIR})
