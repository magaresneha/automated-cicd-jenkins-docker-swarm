pipeline {

    agent any

    parameters {

        choice(
            name: 'APPLICATION',
            choices: [
                'internet-banking',
                'mobile-banking',
                'insurance',
                'loan'
            ],
            description: 'Select the application to build and deploy'
        )
    }

    environment {
        DOCKERHUB_USERNAME = 'snehamagare'
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                    -t ${DOCKERHUB_USERNAME}/${APPLICATION}:${IMAGE_TAG} \
                    ./applications/${APPLICATION}
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USER" \
                        --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                    docker push \
                    ${DOCKERHUB_USERNAME}/${APPLICATION}:${IMAGE_TAG}
                '''
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                sh '''
                    docker stack deploy \
                    -c docker-compose.yml \
                    banking-stack
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    docker stack services banking-stack
                '''
            }
        }
    }

    post {
        success {
            echo "CI/CD pipeline completed successfully."
        }

        failure {
            echo "CI/CD pipeline failed. Check the stage logs."
        }
    }
}
