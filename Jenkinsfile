pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-docker-app:latest .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker rm -f jenkins-docker-app || true
                    docker run -d --name jenkins-docker-app -p 8080:80 jenkins-docker-app:latest
                '''
            }
        }
    }
}
