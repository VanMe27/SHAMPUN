pipeline {
    agent any
    
    environment {
        // Явное указание путей (измените под свою систему)
        VS_PATH = "C:\\Program Files\\Microsoft Visual Studio\\2022\\Community"
        CMAKE_PATH = "C:\\Program Files\\CMake\\bin\\cmake.exe"
    }
    
    stages {
        stage('Verify Tools') {
            steps {
                script {
                    // Проверка наличия CMake
                    bat """
                        @echo off
                        if exist "${env.CMAKE_PATH}" (
                            echo CMake найден: ${env.CMAKE_PATH}
                        ) else (
                            echo ОШИБКА: CMake не найден! Установите с https://cmake.org/download/
                            exit 1
                        )
                    """
                    
                    // Проверка наличия Visual Studio
                    bat """
                        @echo off
                        if exist "${env.VS_PATH}\\VC\\Auxiliary\\Build\\vcvarsall.bat" (
                            echo Visual Studio найден: ${env.VS_PATH}
                        ) else (
                            echo ОШИБКА: Visual Studio не найдена! Установите Community версию
                            exit 1
                        )
                    """
                    
                    // Проверка файлов проекта
                    bat """
                        @echo off
                        dir /b CMakeLists.txt || (
                            echo ОШИБКА: CMakeLists.txt не найден!
                            exit 1
                        )
                        dir /b Shampoo.cpp || (
                            echo ОШИБКА: Shampoo.cpp не найден!
                            exit 1
                        )
                    """
                }
            }
        }
        
        stage('Generate Project') {
            steps {
                bat """
                    @echo off
                    call "${env.VS_PATH}\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                    mkdir build
                    cd build
                    "${env.CMAKE_PATH}" -G "Visual Studio 17 2022" ..
                """
            }
        }
        
        stage('Build') {
            steps {
                bat """
                    @echo off
                    call "${env.VS_PATH}\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                    cd build
                    "${env.CMAKE_PATH}" --build . --config Release
                """
            }
        }
        
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'build/Release/shampoo.exe', fingerprint: true
            }
        }
    }
    
    post {
        failure {
            echo "ДЛЯ ИСПРАВЛЕНИЯ:"
            echo "1. Убедитесь, что установлены:"
            echo "   - Visual Studio 2022 Community с C++"
            echo "   - CMake 3.15+"
            echo "2. Проверьте пути в environment{} в Jenkinsfile"
            echo "3. Запустите Jenkins от имени администратора"
            echo "4. Проверьте наличие CMakeLists.txt и Shampoo.cpp"
        }
    }
}