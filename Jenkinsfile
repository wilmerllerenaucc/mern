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

        stage('Construir imagen Docker local en Minikube') {
            steps {
                powershell '''
                minikube -p minikube docker-env --shell powershell | Invoke-Expression
                docker build -t $env:IMAGE_NAME .
                '''
            }
        }

        stage('Desplegar en Minikube') {
            steps {
                powershell '''
                minikube -p minikube docker-env --shell powershell | Invoke-Expression
                $env:KUBECONFIG="$env:USERPROFILE\\.kube\\config"

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
            echo '✅ Pipeline completado con éxito. Imagen construida y manifiestos desplegados.'
        }
    }
}
