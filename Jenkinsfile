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

        stage('Deploy with Ansible') {
            environment {
                ANSIBLE_HOST_KEY_CHECKING = 'False'
            }

            steps {
                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'ec2-ssh-key',
                        keyFileVariable: 'SSH_KEY',
                        usernameVariable: 'SSH_USER'
                    )
                ]) {
                    sh '''
                        chmod 600 "$SSH_KEY"

                        ansible-playbook \
                          -i ansible/inventory \
                          ansible/deploy.yml \
                          --private-key "$SSH_KEY"
                    '''
                }
            }
        }

        stage('Health Check') {
            steps {
                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'ec2-ssh-key',
                        keyFileVariable: 'SSH_KEY',
                        usernameVariable: 'SSH_USER'
                    )
                ]) {
                    sh '''
                        chmod 600 "$SSH_KEY"

                        ssh -o StrictHostKeyChecking=no \
                            -i "$SSH_KEY" \
                            "$SSH_USER@13.127.163.14" \
                            "curl -f http://localhost:5000/health"
                    '''
                }
            }
        }
    }
}