pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git '<your-repo-link>'
            }
        }

        stage('Build') {
            steps {
                bat 'docker build -t exp8-app .'
            }
        }

        stage('Test') {
            steps {
                bat '''
                docker run -d -p 7070:80 --name test-container exp8-app
                timeout /t 5
                curl http://localhost:7070
                docker rm -f test-container
                '''
            }
        }

        stage('Deploy to Dev') {
            steps {
                bat '''
                docker rm -f dev-container || true
                docker run -d -p 8082:80 --name dev-container exp8-app
                '''
            }
        }

        stage('Deploy to Prod') {
            steps {
                bat '''
                docker rm -f prod-container || true
                docker run -d -p 9090:80 --name prod-container exp8-app
                '''
            }
        }
    }
}