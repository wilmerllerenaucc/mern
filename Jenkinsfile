pipeline {
  agent any

  environment {
    IMAGE_NAME = "mern-crud-app"
  }

  stages {
    stage('Construir imagen Docker local') {
      steps {
        sh 'docker build -t mern-crud-app .'
      }
    }

    stage('Desplegar en Minikube') {
      steps {
        sh 'kubectl apply -f secret.yaml'
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline completado con éxito.'
    }
    failure {
      echo '❌ Hubo un error en el pipeline.'
    }
  }
}
