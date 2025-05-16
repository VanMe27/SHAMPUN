pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Простая компиляция C++ файла напрямую через cl (MSVC)
                    bat """
                        dir /b /s *.cpp  # Выводим список всех CPP файлов
                        cl /EHsc /Fe:shampoo.exe Shampoo.cpp
                    """
                }
            }
        }
        
        stage('Archive') {
            steps {
                // Архивируем полученный exe-файл
                archiveArtifacts artifacts: 'shampoo.exe', fingerprint: true
            }
        }
    }
    
    post {
        always {
            echo "Сборка завершена с статусом: ${currentBuild.currentResult}"
        }
        failure {
            echo "Сборка провалилась. Проверьте:"
            echo "1. Наличие Shampoo.cpp в корне проекта"
            echo "2. Установлен ли Visual Studio Build Tools"
            echo "3. Добавлен ли компилятор cl в PATH"
        }
    }
}