CMAKE_MINIMUM_REQUIRED(VERSION 3.20)
set(PROJECT Mistry)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_BUILD_PARALLEL_LEVEL 16)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


project(${PROJECT} C CXX)

add_subdirectory(${CMAKE_SOURCE_DIR}/dependencies/glfw)


include_directories(${CMAKE_SOURCE_DIR}/dependencies
                    ${CMAKE_SOURCE_DIR}/include
                    ${CMAKE_SOURCE_DIR}/dependencies/imgui
                    ${CMAKE_SOURCE_DIR}/dependencies/glad
)

file(GLOB_RECURSE SOURCES
    ${CMAKE_SOURCE_DIR}/src/*.cpp
    ${CMAKE_SOURCE_DIR}/dependencies/glad/*.c
    ${CMAKE_SOURCE_DIR}/dependencies/imgui/*.cpp
)



add_executable(${PROJECT} ${SOURCES})

if (WIN32)
  target_link_libraries(${PROJECT} glfw opengl32.lib)
elseif (UNIX)
  target_link_libraries(${PROJECT} glfw GL)
else()
    message(STATUS "Unknown platform")
endif()

# After building, copy compile_commands.json to the root folder
add_custom_target(copy-compile-commands ALL
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
    ${CMAKE_BINARY_DIR}/compile_commands.json ${CMAKE_SOURCE_DIR}/compile_commands.json
)
