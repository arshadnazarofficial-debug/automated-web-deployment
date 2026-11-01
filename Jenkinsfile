
pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                echo 'Retrieving project source code'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker build -f Dockerfile.website1 -t arshad4doc/web1_repo:v1 .'
                sh 'docker build -f Dockerfile.website2 -t arshad4doc/web2_repo:v1 .'
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push arshad4doc/web1_repo:v1
                        docker push arshad4doc/web2_repo:v1
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Using Docker Compose') {
            steps {
                sh 'docker compose pull'
                sh 'docker compose up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker compose ps'
            }
        }
    }
}
