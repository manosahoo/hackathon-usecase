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

    withCredentials([string(credentialsId: 'gcp-token', variable: 'eyJhbGciOiJSUzI1NiIsImtpZCI6Inc5QjkyLTJxTmhWSHhwc0hGQnRZSXR2ZXhuMEw5Nk1NU2NYRVlEUktoaTAifQ.eyJhdWQiOlsiaHR0cHM6Ly9jb250YWluZXIuZ29vZ2xlYXBpcy5jb20vdjEvcHJvamVjdHMvcHJvamVjdC1lMTAyYjRmYy1kYjFjLTRkNjgtOWYxL2xvY2F0aW9ucy91cy1jZW50cmFsMS9jbHVzdGVycy9qZW5raW4taGFja2F0aG9uLXVzZWNhc2UiXSwiZXhwIjoxNzc4MDQ4MjQ3LCJpYXQiOjE3NzgwNDQ2NDcsImlzcyI6Imh0dHBzOi8vY29udGFpbmVyLmdvb2dsZWFwaXMuY29tL3YxL3Byb2plY3RzL3Byb2plY3QtZTEwMmI0ZmMtZGIxYy00ZDY4LTlmMS9sb2NhdGlvbnMvdXMtY2VudHJhbDEvY2x1c3RlcnMvamVua2luLWhhY2thdGhvbi11c2VjYXNlIiwianRpIjoiM2JlYWRhNmEtODZmOC00MjE1LWFlMDYtYjUxNGIyMWU1MmIxIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJkZWZhdWx0Iiwic2VydmljZWFjY291bnQiOnsibmFtZSI6ImhhY2thdGhvbiIsInVpZCI6Ijc3ZmQ4MTE5LTVlNzUtNDhlNy05M2MwLTEzMWUyMjE1ZmZjMiJ9fSwibmJmIjoxNzc4MDQ0NjQ3LCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDpoYWNrYXRob24ifQ.Whiw4AeAt-XSRCh8eNdzO6sHGaoT08tSnbbyOjNHUbkqDzSBForlaUPKtlEYf9SR3UvMhaWHID1H9VHaTmXUceQseOT2TCUI4YJ5YDR5v5AF2nYOQgAseYJoTeXIy3g73QsHLw_rTpUHV06jHZPR5U8QOVy861ofzyeHXvlC5ZDe6dT5SHUKefStBj_pbV29PwJ3f9gk11ZQVmqvXZIQLEfle3XvKd3llJqFItpoZ18z9YbN5BkmQYwRy4D-YKt-iERksvTN5sLQ5mIR5o0KE-Rxb4K-V-jl7H2mKqN')]) {
    sh '''
    curl -H "Authorization: Bearer $TOKEN" \
    https://container.googleapis.com/v1/projects
    '''
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
