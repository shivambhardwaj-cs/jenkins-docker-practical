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

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh '''
                        docker tag jenkins-docker-app:latest "$DOCKER_USER/jenkins-docker-practical:latest"
                        echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push "$DOCKER_USER/jenkins-docker-practical:latest"
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Locally') {
            steps {
                sh '''
                    docker rm -f jenkins-docker-app || true
                    docker run -d --name jenkins-docker-app -p 8081:80 jenkins-docker-app:latest
                '''
            }
        }
    }
}
