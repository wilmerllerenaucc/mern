pipeline {
    agent any

    environment {
        // Nombre de la imagen
        IMAGE_NAME = 'mern-crud-app'
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/wilmerllerenaucc/mern.git'
            }
        }

        stage('Construir imagen Docker local en Minikube') {
            steps {
                bat '''
                @echo off
                rem Configurar entorno Docker para usar el daemon de Minikube
                for /f "delims=" %%i in ('minikube docker-env --shell=cmd') do call %%i
                docker build -t %IMAGE_NAME% .
                '''
            }
        }

        stage('Desplegar en Minikube (opcional)') {
            steps {
                echo 'Aquí podrías aplicar manifests de Kubernetes con kubectl si lo deseas.'
                // ejemplo:
                // bat 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }

    post {
        failure {
            echo '❌ Hubo un error en el pipeline.'
        }
        success {
            echo '✅ Pipeline completado con éxito.'
        }
    }
}
