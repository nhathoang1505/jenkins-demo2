pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'javac WebServer.java'
            }
        }
        stage('Deploy') {
            steps {
                bat '''
                    # Kill old process on 8081 (nếu có)
                    fuser -k 8081/tcp || true
                    
                    # Chạy server ở background, log ra file
                    nohup java WebServer > server.log 2>&1 &
                '''
            }
        }
    }
}


