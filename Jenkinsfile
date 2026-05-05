pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
              git branch: 'main', url: 'https://github.com/manosahoo/hackathon-usecase.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t gcr.io/project-e102b4fc-db1c-4d68-9f1/node-app:$BUILD_NUMBER .'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push gcr.io/project-e102b4fc-db1c-4d68-9f1/node-app:$BUILD_NUMBER'
            }
        }

        stage('Deploy to GKE') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
