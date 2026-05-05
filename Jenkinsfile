pipeline {
  agent {
    kubernetes {
      label 'jenkins-gcloud'
      defaultContainer 'gcloud'

      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: gcloud
    image: google/cloud-sdk:latest
    command:
    - cat
    tty: true
"""
    }
  }

  stages {

    stage('Authenticate GCP') {
      steps {
        withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
          sh '''
          gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
          gcloud config set project project-e102b4fc-db1c-4d68-9f1
          '''
        }
      }
    }

    stage('Build Image') {
      steps {
        withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
          sh '''
          gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS

          gcloud builds submit \
            --tag gcr.io/project-e102b4fc-db1c-4d68-9f1/node-app:$BUILD_NUMBER
          '''
        }
      }
    }
  }
}
