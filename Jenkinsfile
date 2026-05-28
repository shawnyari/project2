pipeline {
    agent any

    environment {
        IMAGE_NAME = "food-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/shawnyari/project2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f food-container || true
                docker run -d --name food-container -p 5000:5000 $IMAGE_NAME
                '''
            }
        }

        stage('Test Application') {
            steps {
                sh 'curl localhost:5000'
            }
        }
    }
}
