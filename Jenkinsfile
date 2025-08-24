pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/nhathoang1505/jenkins-demo2.git', branch: 'main'
      }
    }

    stage('Build') {
      steps {
        bat '''
        javac -d . src\\WebServer.java
        '''
      }
    }

    stage('Run') {
      steps {
        bat 'start /B java WebServer'
      }
    }

    stage('Test') {
      steps {
        bat '''
        ping -n 5 127.0.0.1 >nul
        curl http://localhost:8081
        '''
      }
    }
  }

  post {
    always {
      bat '''
      @echo off
      setlocal
      echo Cleaning up port 8081...
      for /f "tokens=5" %%p in ('netstat -ano ^| findstr /R /C:"TCP.*:8081.*LISTENING"') do (
        taskkill /PID %%p /F /T >nul 2>&1
      )
      exit /b 0
      '''
    }
  }
}
