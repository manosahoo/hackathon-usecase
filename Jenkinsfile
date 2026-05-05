pipeline {
     agent {
    docker {
      image 'google/cloud-sdk:latest'
    }

    stages {

        stage('Checkout') {
            steps {
              git branch: 'main', url: 'https://github.com/manosahoo/hackathon-usecase.git'
            }
        }

        
stage('Build Image') {
    steps {
        sh '''
        gcloud builds submit \
          --tag gcr.io/project-e102b4fc-db1c-4d68-9f1/node-app:$BUILD_NUMBER
        '''
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
