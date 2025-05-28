pipeline {
  agent any

  environment {
    IMAGE_NAME = "wilmerllerenaucc/mern-crud-app"
  }

  stages {
    stage('Clonar repositorio') {
      steps {
        git 'https://github.com/wilmerllerenaucc/mern.git'
      }
    }

    stage('Construir imagen Docker') {
      steps {
        script {
          dockerImage = docker.build("${IMAGE_NAME}:latest")
        }
      }
    }

    stage('Ejecutar pruebas (opcional)') {
      steps {
        echo 'No hay pruebas configuradas aún.'
        // Si las agregas, podrías hacer:
        // sh 'npm install && npm test'
      }
    }

    stage('Publicar en DockerHub') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds', 
          usernameVariable: 'USER', 
          passwordVariable: 'PASS'
        )]) {
          script {
            sh 'echo $PASS | docker login -u $USER --password-stdin'
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Fin') {
      steps {
        echo '✅ Proceso completado. Imagen lista en DockerHub.'
      }
    }
  }

  post {
    failure {
      echo '❌ Error en alguna etapa del pipeline.'
    }
  }
}
