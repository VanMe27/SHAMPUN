pipeline {
    agent any

    environment {
        CMAKE_PATH = "C:\\Program Files\\CMake\\bin\\cmake.exe"
        VS_PATH = '"C:\\Program Files\\Microsoft Visual Studio\\2022\\Professional\\Common7\\Tools\\VsDevCmd.bat"'
    }

    stages {
        stage('Verify Tools') {
            steps {
                script {
                    // Проверка наличия CMake
                    bat """
                        @echo off
                        if exist ${CMAKE_PATH} (
                            echo CMake найден: ${CMAKE_PATH}
                        ) else (
                            echo ОШИБКА: CMake не найден! Установите с https://cmake.org/download/
                            exit 1
                        )
                    """

                    // Проверка наличия Visual Studio (VsDevCmd.bat)
                    bat """
                        @echo off
                        if exist ${VS_PATH} (
                            echo Visual Studio найден: ${VS_PATH}
                        ) else (
                            echo ОШИБКА: Visual Studio не найдена! Проверьте путь VS_PATH
                            exit 1
                        )
                    """

                    // Проверка наличия исходных файлов
                    bat """
                        @echo off
                        if not exist CMakeLists.txt (
                            echo ОШИБКА: CMakeLists.txt не найден!
                            exit 1
                        )
                        if not exist Shampoo.cpp (
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
                    call ${VS_PATH}
                    mkdir build
                    cd build
                    ${CMAKE_PATH} -G "Visual Studio 17 2022" ..
                """
            }
        }

        stage('Build') {
            steps {
                bat """
                    @echo off
                    call ${VS_PATH}
                    cd build
                    ${CMAKE_PATH} --build . --config Release
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
            echo "   - Visual Studio 2022 Professional с C++"
            echo "   - CMake 3.15+"
            echo "2. Проверьте пути в environment{} в Jenkinsfile"
            echo "3. Запустите Jenkins от имени администратора"
            echo "4. Проверьте наличие CMakeLists.txt и Shampoo.cpp"
        }
    }
}
