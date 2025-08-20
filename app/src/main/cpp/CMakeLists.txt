cmake_minimum_required(VERSION 3.22.1)

project(byedpi_native)

# Пути к исходникам
set(BYE_DPI_PATH ${CMAKE_CURRENT_SOURCE_DIR}/byedpi)

# Берём все .c файлы, кроме Windows-specific
file(GLOB BYE_DPI_SRC ${BYE_DPI_PATH}/*.c)
list(REMOVE_ITEM BYE_DPI_SRC ${BYE_DPI_PATH}/win_service.c)

# Создаём библиотеку
add_library(byedpi SHARED ${BYE_DPI_SRC} native-lib.c)

# Добавляем include-директорию
target_include_directories(byedpi PRIVATE ${BYE_DPI_PATH})

# Компиляторные опции
target_compile_options(byedpi PRIVATE
    -std=c99
    -O2
    -Wall
    -Wno-unused
    -Wextra
    -Wno-unused-parameter
    -pedantic
)

# Определения для Android
target_compile_definitions(byedpi PRIVATE ANDROID_APP)

# Линки на Android библиотеки
target_link_libraries(byedpi PRIVATE android log)
