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
                sh 'echo "Running application checks..."'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    cd app
                    docker build -t divyamaghade/devops-phase1-app:1.0 .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Docker Hub push will be configured next'
            }
        }

        stage('Deploy using Ansible') {
            steps {
                echo 'Ansible deployment will be configured next'
            }
        }

        stage('Health Check') {
            steps {
                echo 'Health check will be configured next'
            }
        }
    }
}
