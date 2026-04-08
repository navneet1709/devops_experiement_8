pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'docker build -t exp8-app .'
            }
        }

        stage('Test') {
            steps {
                bat '''
                docker rm -f test-container 2>nul || ver >nul
                docker run -d -p 7070:80 --name test-container exp8-app
                timeout /t 5
                curl http://localhost:7070
                docker rm -f test-container
                '''
            }
        }

        stage('Deploy Dev') {
            steps {
                bat '''
                docker rm -f dev-container 2>nul || ver >nul
                docker run -d -p 8081:80 --name dev-container exp8-app
                '''
            }
        }

        stage('Deploy Prod') {
            steps {
                bat '''
                docker rm -f prod-container 2>nul || ver >nul
                docker run -d -p 9090:80 --name prod-container exp8-app
                '''
            }
        }
    }
}