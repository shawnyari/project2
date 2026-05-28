pipeline {
    agent any

    environment {
        IMAGE_NAME = "yarishawn7/food-app"
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

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Run Container') {
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
