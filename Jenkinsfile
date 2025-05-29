pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mern-crud-app'
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/wilmerllerenaucc/mern.git'
            }
        }

        stage('Ejecutar tests con Jest') {
            steps {
                bat '''
                cd backend
                call npm install
                call npm test
                '''
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

        stage('Desplegar en Minikube') {
            steps {
                bat '''
                @echo off
                kubectl apply -f secret.yaml
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }

    post {
        failure {
            echo '❌ Hubo un error en el pipeline.'
        }
        success {
            echo '✅ Pipeline completado con éxito. Tests y despliegue aplicados.'
        }
    }
}
