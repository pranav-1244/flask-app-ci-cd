pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "your_dockerhub_username"
        DOCKERHUB_PASSWORD = "your_dockerhub_password"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/pranav-1244/flask-app-ci-cd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKERHUB_USERNAME}/flask-app:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                echo "${DOCKERHUB_PASSWORD}" | docker login -u "${DOCKERHUB_USERNAME}" --password-stdin
                docker push ${DOCKERHUB_USERNAME}/flask-app:latest
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop flask-app || true
                docker rm flask-app || true
                docker run -d -p 5000:5000 --name flask-app ${DOCKERHUB_USERNAME}/flask-app:latest
                '''
            }
        }
    }
}
