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
                bat '''
                if not exist build mkdir build
                javac -d build src\\WebServer.java
                '''
            }
        }

        stage('Deploy (start server)') {
            steps {
                bat '''
                if not exist deploy mkdir deploy
                REM chạy nền, log vào deploy\\webserver.log
                start "" /b cmd /c "java -cp build WebServer > deploy\\webserver.log 2>&1"
                '''
            }
        }

        stage('Verify') {
            steps {
                bat '''
                REM Đợi server chạy lên
                timeout /t 5 /nobreak
                REM Test bằng curl
                curl http://localhost:8081
                '''
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            bat '''
            REM Dọn dẹp process java chạy nền
            for /f "tokens=5" %%a in ('netstat -ano ^| findstr :8081') do taskkill /PID %%a /F
            '''
        }
    }
}
