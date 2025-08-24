pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<tên của bạn>/jenkins-demo1.git'
            }
        }

        stage('Build') {
            steps {
                bat '''
                if not exist build mkdir build
                javac -d build src\\WebServer.java
                '''
            }
        }

        stage('Stop previous on 8081') {
            steps {
                bat '''
                for /f "tokens=5" %%a in ('netstat -ano ^| findstr :8081 ^| findstr LISTENING') do taskkill /PID %%a /F
                exit /b 0
                '''
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                if not exist deploy mkdir deploy
                start "" /b cmd /c "java -cp build WebServer > deploy\\webserver.log 2>&1"
                '''
            }
        }

        stage('Verify') {
            steps {
                bat '''
                timeout /t 3 > NUL
                curl -s http://localhost:8081 > deploy\\check.txt
                type deploy\\check.txt
                findstr /C:"Hello from Jenkins CI/CD" deploy\\check.txt
                '''
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD OK — mở http://localhost:8081 để xem.'
        }
        failure {
            echo '❌ Lỗi rồi — xem log ở Console Output.'
        }
    }
}
