pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Jenkins pipeline started successfully"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                      -t divyamaghade/devops-phase1-app:1.0 \
                      ./app
                '''
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh '''
                    docker images | grep devops-phase1-app
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                          -u "$DOCKER_USERNAME" \
                          --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                    docker push divyamaghade/devops-phase1-app:1.0
                '''
            }
        }
    }
}
