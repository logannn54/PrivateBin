pipeline {
    agent any
    environment {
        APP_NAME = 'your_app'
        DOCKER_IMAGE = 'myregistry.com/myapp:latest'
    }
    stages {
        stage('Checkout') {
            steps {
                // Récupération du code source depuis le dépôt GitHub
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                // Construction de l'image Docker
                sh 'docker build -t ${APP_NAME}:latest .'
            }
        }
        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to registry...'
                // Tag de l'image Docker et envoi vers le registre
                sh 'docker tag ${APP_NAME}:latest ${DOCKER_IMAGE}'
                sh 'docker push ${DOCKER_IMAGE}'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application using Docker on local server...'
                // Déploiement de l'image Docker sur le serveur local
                sh """
                docker pull ${DOCKER_IMAGE} &&
                docker stop ${APP_NAME} || true &&
                docker rm ${APP_NAME} || true &&
                docker run -d --name ${APP_NAME} -p 80:80 ${DOCKER_IMAGE}
                """
            }
        }
    }
}
