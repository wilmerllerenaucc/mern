pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mern-crud-app'
        // Ruta al kubeconfig del usuario SYSTEM (donde Jenkins normalmente se ejecuta en Windows)
        KUBECONFIG = 'C:\\Windows\\System32\\config\\systemprofile\\.kube\\config'
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/wilmerllerenaucc/mern.git'
            }
        }

        stage('Verificar conexión con Kubernetes') {
            steps {
                bat '''
                echo Verificando conexión con Minikube...
                kubectl config current-context
                kubectl get nodes
                '''
            }
        }

        stage('Construir imagen Docker en Minikube') {
            steps {
                bat '''
                rem Configurar entorno Docker para usar Minikube
                call minikube -p minikube docker-env --shell=cmd > minikube_env.bat
                call minikube_env.bat

                rem Construir imagen
                docker build -t %IMAGE_NAME% .
                '''
            }
        }

        stage('Eliminar recursos anteriores') {
            steps {
                bat '''
                echo Eliminando recursos antiguos (si existen)...
                kubectl delete -f secret.yaml --ignore-not-found
                kubectl delete -f deployment.yaml --ignore-not-found
                kubectl delete -f service.yaml --ignore-not-found
                '''
            }
        }

        stage('Desplegar manifiestos') {
            steps {
                bat '''
                echo Aplicando manifiestos de Kubernetes...
                kubectl apply -f secret.yaml --validate=false
                kubectl apply -f deployment.yaml --validate=false
                kubectl apply -f service.yaml --validate=false
                '''
            }
        }
    }

    post {
        failure {
            echo '❌ Falló el pipeline. Revisa los logs de Jenkins para detalles.'
        }
        success {
            echo '✅ Pipeline completado exitosamente. Imagen desplegada en Minikube.'
        }
    }
}
