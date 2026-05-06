pipeline {
    agent any

    environment {
        PROJECT_ID = "project-e102b4fc-db1c-4d68-9f1"
        IMAGE = "us-central1-docker.pkg.dev/${PROJECT_ID}/repo/my-app"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/manosahoo/hackathon-usecase.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t $IMAGE:$TAG ."
            }
        }

        stage('Authenticate (NO KEY)') {
            steps {
                sh '''
                gcloud auth list
                gcloud config set project $PROJECT_ID
                gcloud auth configure-docker us-central1-docker.pkg.dev -q
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh "docker push $IMAGE:$TAG"
            }
        }

        stage('Deploy to GKE') {
            steps {
                sh '''
                kubectl set image deployment/my-app \
                my-app=$IMAGE:$TAG
                '''
            }
        }
    }
}
