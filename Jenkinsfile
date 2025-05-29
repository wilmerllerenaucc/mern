pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mern-crud-app'
        KUBECONFIG = 'C:\\Windows\\System32\\config\\systemprofile\\.kube\\config' // Ruta del usuario Jenkins
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/wilmerllerenaucc/mern.git'
            }
        }

        stage('Construir imagen en Minikube') {
            steps {
                bat '''
                call minikube -p minikube docker-env --shell=cmd > minikube_env.bat
                call minikube_env.bat
                docker build -t %IMAGE_NAME% .
                '''
            }
        }

        stage('Desplegar en Minikube') {
            steps {
                bat '''
                kubectl apply -f secret.yaml --validate=false
                kubectl apply -f deployment.yaml --validate=false
                kubectl apply -f service.yaml --validate=false
                '''
            }
        }
    }

    post {
        failure {
            echo '❌ Hubo un error en el pipeline.'
        }
        success {
            echo '✅ Despliegue exitoso en Minikube.'
        }
    }
}
