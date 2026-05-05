pipeline {
    agent any

    environment {
        IMAGE_NAME = "manosahoo/hackathon-usecase"
        TAG = "${BUILD_NUMBER}"
    }

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/manosahoo/hackathon-usecase.git''
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$TAG'
                sh 'docker tag $IMAGE_NAME:$TAG $IMAGE_NAME:latest'
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploy to Kubernetes"
                // sh 'kubectl apply -f deployment.yaml'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        success {
            echo "Build & Deployment Successful 🎉"
        }
        failure {
            echo "Build Failed ❌"
        }
    }
}
