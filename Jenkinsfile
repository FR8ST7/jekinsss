pipeline {
  agent any
  environment {
    IMAGE = "<dockerhub-username>/myapp:latest"
  }
  stages {
    stage('Clone') {
      steps { git 'https://github.com/<username>/<repo>.git' }
    }
    stage('Build') {
      steps { sh 'docker build -t $IMAGE .' }
    }
    stage('Push') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin && docker push $IMAGE'
        }
      }
    }
    stage('Deploy') {
      steps { sh 'docker run -d -p 5000:5000 $IMAGE' }
    }
  }
}