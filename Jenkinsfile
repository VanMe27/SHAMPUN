pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Проверяем наличие CMakeLists.txt
                    bat '''
                        dir
                        if exist CMakeLists.txt (
                            echo "CMakeLists.txt найден"
                            mkdir build
                            cd build
                            cmake -G "Visual Studio 17 2022" ..
                            cmake --build . --config Release
                        ) else (
                            echo "ОШИБКА: CMakeLists.txt не найден в корне проекта!"
                            exit 1
                        )
                    '''
                }
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'build/Release/**/*.exe', allowEmptyArchive: true
        }
        failure {
            echo "Сборка провалилась. Проверьте наличие CMakeLists.txt в корне репозитория."
        }
    }
}