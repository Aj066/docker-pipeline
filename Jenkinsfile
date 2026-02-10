pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "aj000/amazon-nginx"
        DOCKER_TAG = "1.0"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/Aj066/docker-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
        success {
            echo 'Docker image built and pushed successfully 🎉'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
