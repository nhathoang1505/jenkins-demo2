pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/nhathoang1505/jenkins-demo2.git'
            }
        }

        stage('Build') {
            steps {
                bat 'javac -d . src/WebServer.java'
            }
        }

        stage('Run') {
            steps {
                // Start WebServer in background
                bat 'start /B java WebServer'
                // Wait 5s cho server khởi động
                bat 'ping -n 5 127.0.0.1 >nul'
            }
        }

        stage('Test') {
            steps {
                bat 'curl http://localhost:8081'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Kill đúng process chiếm port 8081 (tránh PID = 0)
            bat '''
            for /F "tokens=5" %%a in ('netstat -ano ^| findstr :8081') do (
                if not "%%a"=="0" taskkill /PID %%a /F
            )
            '''
        }
    }
}
