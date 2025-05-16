pipeline {
    agent any
    
    environment {
        // Путь к Visual Studio (автопоиск)
        VS_PATH = bat(script: "call \"C:\\Program Files\\Microsoft Visual Studio\\Installer\\vswhere.exe\" -property installationPath", returnStdout: true).trim()
    }
    
    stages {
        stage('Check Environment') {
            steps {
                script {
                    // Проверка наличия CMakeLists.txt
                    bat '''
                        @echo off
                        dir /b CMakeLists.txt || (
                            echo ОШИБКА: CMakeLists.txt не найден в корне проекта!
                            exit 1
                        )
                    '''
                    
                    // Проверка наличия исходного файла
                    bat '''
                        @echo off
                        dir /b Shampoo.cpp || (
                            echo ОШИБКА: Shampoo.cpp не найден!
                            exit 1
                        )
                    '''
                }
            }
        }
        
        stage('Generate Build System') {
            steps {
                bat """
                    @echo off
                    call "${env.VS_PATH}\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                    mkdir build
                    cd build
                    cmake -G "Visual Studio 17 2022" ..
                """
            }
        }
        
        stage('Build') {
            steps {
                bat """
                    @echo off
                    call "${env.VS_PATH}\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                    cd build
                    cmake --build . --config Release
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
            echo "СБОРКА ПРОВАЛИЛАСЬ. ВАЖНЫЕ ШАГИ:"
            echo "1. Установите Visual Studio 2022 с компонентом 'C++'"
            echo "2. Установите CMake (https://cmake.org/download/)"
            echo "3. Запустите Jenkins от имени администратора"
            echo "4. Убедитесь, что в репозитории есть CMakeLists.txt и Shampoo.cpp"
        }
    }
}