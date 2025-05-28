pipeline {
    agent any

    environment {
        DOCKER_HOST = 'tcp://127.0.0.1:2375' // Por si usas Docker remoto con Minikube
        IMAGE_NAME = 'mern-crud-app'
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git url: 'https://github.com/wilmerllerenaucc/mern.git', branch: 'main'
            }
        }

        stage('Construir imagen Docker local') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Desplegar en Minikube') {
            steps {
                bat 'kubectl apply -f secret.yaml'
                bat 'kubectl apply -f mongo-deployment.yaml'
                bat 'kubectl apply -f mongo-service.yaml'
                bat 'kubectl apply -f app-deployment.yaml'
                bat 'kubectl apply -f app-service.yaml'
            }
        }
    }

    post {
        failure {
            echo '❌ Hubo un error en el pipeline.'
        }
        success {
            echo '✅ Despliegue completado exitosamente.'
        }
    }
}
