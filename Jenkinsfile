pipeline {
  agent any

  environment {
    IMAGE_NAME = "mern-crud-app"
  }

  stages {
    stage('Clonar repositorio') {
      steps {
        git 'https://github.com/wilmerllerenaucc/mern.git'
      }
    }

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
          // Opcional: asegúrate de que Jenkins tenga el contexto minikube activo
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
