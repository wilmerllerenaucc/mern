pipeline {
  agent any

  environment {
    IMAGE_NAME = "mern-crud-app"
  }

  stages {
    stage('Construir imagen Docker local') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}:latest")
        }
      }
    }

    stage('Desplegar en Minikube') {
      steps {
        script {
          sh 'kubectl apply -f secret.yaml'
          sh 'kubectl apply -f deployment.yaml'
          sh 'kubectl apply -f service.yaml'
        }
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline completado. Despliegue en Minikube exitoso.'
    }
    failure {
      echo '❌ Hubo un error en el pipeline.'
    }
  }
}
