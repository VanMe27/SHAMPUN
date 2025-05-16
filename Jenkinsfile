pipeline {
    agent any
    
    environment {
        // Путь к компилятору Visual Studio (измените под вашу версию!)
        VS_PATH = "C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\VC\\Auxiliary\\Build"
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    bat """
                        @echo off
                        call "%VS_PATH%\\vcvarsall.bat" x64
                        dir /b *.cpp
                        cl /EHsc /Fe:shampoo.exe Shampoo.cpp
                    """
                }
            }
        }
        
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'shampoo.exe', fingerprint: true
            }
        }
    }
    
    post {
        always {
            echo "Результат сборки: ${currentBuild.currentResult}"
        }
        failure {
            echo "Проверьте следующее:"
            echo "1. Файл Shampoo.cpp должен быть в корне репозитория"
            echo "2. Установите Visual Studio 2022 Build Tools"
            echo "3. Обновите путь к VS_PATH в Jenkinsfile если требуется"
        }
    }
}