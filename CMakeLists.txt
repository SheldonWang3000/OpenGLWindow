cmake_minimum_required (VERSION 2.6)
project (OpenGLWindow)
# The version number.
set (OpenGLWindow_VERSION_MAJOR 1)
set (OpenGLWindow_VERSION_MINOR 0)

include_directories ("${PROJECT_SOURCE_DIR}/glfw")

# GLAD GLI
include_directories ("${PROJECT_SOURCE_DIR}/include")

# GLFW
set (GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set (GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set (GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set (GLFW_INSTALL OFF CACHE BOOL "" FORCE)
add_subdirectory (glfw)
set (EXTRA_LIBS ${EXTRA_LIBS} glfw)

# source & header
file (GLOB SOURCES src/*.*)

add_executable (${PROJECT_NAME} ${SOURCES})

# link glfw
target_link_libraries (${PROJECT_NAME} ${EXTRA_LIBS})
