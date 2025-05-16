pipeline {
    agent any  // Запуск на любом агенте (Windows/Linux)
    
    stages {
        // Этап 1: Получение кода из репозитория
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/VanMe27/SHAMPUN.git'  
            }
        }
        
        // Этап 2: Сборка проекта (используем CMake и make)
        stage('Build') {
            steps {
                script {
                    // Для Windows (MinGW или MSVC)
                    if (isUnix()) {
                        sh '''
                            mkdir -p build
                            cd build
                            cmake ..
                            make
                        '''
                    } else {
                        bat '''
                            mkdir build
                            cd build
                            cmake -G "MinGW Makefiles" ..
                            mingw32-make
                        '''
                    }
                }
            }
        }
        
        // Этап 3: Запуск тестов (если есть)
        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'cd build && ctest --verbose'
                    } else {
                        bat 'cd build && ctest --verbose'
                    }
                }
            }
        }
        
        // Этап 4: Упаковка артефактов (например, .exe или .a)
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'build/**/*.exe,build/**/*.a,build/**/*.so', 
                                fingerprint: true
            }
        }
    }
    
    // Действия после сборки
    post {
        always {
            emailext body: "Сборка ${currentBuild.currentResult}\nПодробности: ${env.BUILD_URL}",
                     subject: "Результат сборки C++ проекта",
                     to: 'ghostprizrak2011@gmail.com'
        }
    }
}