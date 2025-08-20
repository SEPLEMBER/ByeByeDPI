cmake_minimum_required(VERSION 3.22.1)
project(byedpi_native)

# Пусть path/to/byedpi — путь к клону оригинала
set(BYE_DPI_PATH ${CMAKE_CURRENT_SOURCE_DIR}/byedpi_submodule)

file(GLOB BYE_DPI_SRC
    ${BYE_DPI_PATH}/*.c
)
list(REMOVE_ITEM BYE_DPI_SRC ${BYE_DPI_PATH}/win_service.c)

add_library(byedpi SHARED ${BYE_DPI_SRC} native-lib.c)

target_include_directories(byedpi PRIVATE
    ${BYE_DPI_PATH}
)

target_compile_options(byedpi PRIVATE
    -std=c99 -O2 -Wall -Wno-unused -Wextra -Wno-unused-parameter -pedantic
)
target_compile_definitions(byedpi PRIVATE ANDROID_APP)
target_link_libraries(byedpi PRIVATE android log)
