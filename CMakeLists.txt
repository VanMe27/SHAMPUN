cmake_minimum_required(VERSION 3.15)
project(Shampoo)

# Настройка компилятора (MSVC для Windows)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Добавляем исполняемый файл
add_executable(shampoo Shampoo.cpp)

# Для Visual Studio укажите конфигурацию сборки
if(MSVC)
    set_target_properties(shampoo PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/Release
        RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/Release
    )
endif()