pipeline {
    agent any
    
    stages {
        // Этап 1: Получение кода (автоматически через SCM)
        stage('Build') {
            steps {
                script {
                    // Используем MSBuild (Visual Studio)
                    bat """
                        mkdir build
                        cd build
                        cmake -G "Visual Studio 17 2022" ..
                        cmake --build . --config Release
                    """
                }
            }
        }
        
        // Этап 2: Запуск тестов (если есть)
        stage('Test') {
            steps {
                bat 'cd build && ctest --verbose'
            }
        }
    }
    
    post {
        always {
            // Архивируем артефакты (исполняемые файлы)
            archiveArtifacts artifacts: 'build/Release/**/*.exe', fingerprint: true
        }
        failure {
            // Уведомление о неудачной сборке
            mail to: 'ghostprizrak2011@gmail.com',
                 subject: "Сборка ${env.JOB_NAME} провалилась",
                 body: "Подробности: ${env.BUILD_URL}"
        }
    }
}