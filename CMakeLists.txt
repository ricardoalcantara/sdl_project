cmake_minimum_required(VERSION 3.0.0)
project(sdl_project)

set(EXTERNAL "./external")

set(CMAKE_CXX_STANDARD 11)

add_subdirectory(${EXTERNAL}/SDL2-2.0.12)

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic")
endif()

include_directories(src/
                    ${EXTERNAL}/SDL2-2.0.12/include/)

add_executable(${PROJECT_NAME} src/main.cpp)

target_link_libraries(${PROJECT_NAME} SDL2main SDL2)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        # $<TARGET_FILE:SDL2main>
        $<TARGET_FILE:SDL2>
        $<TARGET_FILE_DIR:${PROJECT_NAME}>)
