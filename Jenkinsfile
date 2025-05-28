pipeline {
  agent any

  environment {
    IMAGE_NAME = "mern-crud-app"
  }

  stages {
    stage('Construir imagen Docker local') {
      steps {
        bat 'docker build -t mern-crud-app .'
      }
    }

    stage('Desplegar en Minikube') {
      steps {
        bat 'kubectl apply -f secret.yaml'
        bat 'kubectl apply -f deployment.yaml'
        bat 'kubectl apply -f service.yaml'
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
