cmake_minimum_required(VERSION 3.15)
project(Shampoo)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Подключаем исходники
file(GLOB_RECURSE SOURCES "Shampoo.cpp")

add_executable(shampoo ${SOURCES})

# Настройка путей вывода для Visual Studio
if(MSVC)
    set_target_properties(shampoo PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/Release"
        RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_BINARY_DIR}/Release"
    )
endif()
