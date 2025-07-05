pipeline {
    agent {
        label 'agent-vinod'
    }

    environment {
        IMAGE_NAME = "notes-app:latest"
        DOCKER_REPO = "88151"
    }

    stages {

        stage('Clone') {
            steps {
                echo "Cloning the code..."
                git branch: 'main', url: 'https://github.com/azharism/django-todo-cicd.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "Pushing image to DockerHub..."
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-cred',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh "docker tag ${IMAGE_NAME} ${DOCKER_REPO}/${IMAGE_NAME}"
                    sh "docker push ${DOCKER_REPO}/${IMAGE_NAME}"
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying with Docker Compose...'
                sh 'docker compose up -d'
            }
        }
    }
}
