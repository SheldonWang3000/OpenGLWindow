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

add_library(GLAD "glad/glad.c")
set(EXTRA_LIBS ${EXTRA_LIBS} GLAD)

# source & header
# build source group by folder name
set(SOURCE_ROOT_PATH "${PROJECT_SOURCE_DIR}/src")
file(
    GLOB_RECURSE SOURCES 
    LIST_DIRECTORIES false
    "${SOURCE_ROOT_PATH}/*.c*"
    "${SOURCE_ROOT_PATH}/*.h*"
)

foreach(SOURCE_NAME IN ITEMS ${SOURCES})
    get_filename_component(SOURCE_DIR "${SOURCE_NAME}" PATH)
    file(RELATIVE_PATH SOURCE_RELATIVE_PATH "${SOURCE_ROOT_PATH}" "${SOURCE_DIR}")
    string(REPLACE "/" "\\" GROUP_NAME "${SOURCE_RELATIVE_PATH}")
    if("${SOURCE_DIR}" STREQUAL "${SOURCE_ROOT_PATH}")
        get_filename_component(FILE_EXT "${SOURCE_NAME}" EXT)
        if("${FILE_EXT}" MATCHES "^\\.h+")
            set(GROUP_NAME "Header Files")
        else()
            set(GROUP_NAME "Source Files")
        endif()
        
    endif()
    source_group("${GROUP_NAME}" FILES "${SOURCE_NAME}")
endforeach()

add_executable (${PROJECT_NAME} ${SOURCES})

# link glfw
target_link_libraries (${PROJECT_NAME} ${EXTRA_LIBS})
