pipeline {
    agent any
    
    environment {
        // Автоматическое определение пути к Visual Studio
        VS_PATH = bat(script: "call \"C:\\Program Files\\Microsoft Visual Studio\\Installer\\vswhere.exe\" -property installationPath", returnStdout: true).trim()
    }
    
    stages {
        stage('Setup Environment') {
            steps {
                script {
                    // Проверяем наличие Shampoo.cpp
                    bat '''
                        dir /b Shampoo.cpp || echo Файл Shampoo.cpp не найден! && exit 1
                    '''
                    
                    // Настраиваем окружение Visual Studio
                    bat """
                        @echo off
                        call "${env.VS_PATH}\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                        where cl || echo Компилятор cl.exe не найден! && exit 1
                    """
                }
            }
        }
        
        stage('Build') {
            steps {
                bat 'cl /EHsc /Fe:shampoo.exe Shampoo.cpp'
            }
        }
        
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'shampoo.exe', fingerprint: true
            }
        }
    }
    
    post {
        failure {
            echo "СБОРКА ПРОВАЛИЛАСЬ. ВАЖНО:"
            echo "1. Установите Visual Studio 2022 Build Tools с компонентом 'C++'"
            echo "2. Запустите Jenkins ОТ ИМЕНИ АДМИНИСТРАТОРА (чтобы он видел переменные среды)"
            echo "3. Убедитесь, что файл Shampoo.cpp есть в корне репозитория"
        }
    }
}