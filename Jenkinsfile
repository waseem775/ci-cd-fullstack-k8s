pipeline {
    agent any

    environment {
        BACKEND_IMAGE = 'laravel-backend:latest'
        FRONTEND_IMAGE = 'react-frontend:latest'
        KUBE_CONFIG = '/home/jenkins/.kube/config'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/waseem775/ci-cd-fullstack-k8s.git'
            }
        }

        stage('Build Laravel Backend') {
            steps {
                dir('backend') {
                    sh '''
                    eval $(minikube docker-env)
                    docker build -t $BACKEND_IMAGE .
                    '''
                }
            }
        }

        stage('Run Laravel Tests') {
            steps {
                dir('backend') {
                    sh '''
                    eval $(minikube docker-env)
                    docker run --rm $BACKEND_IMAGE php artisan test
                    '''
                }
            }
        }

        stage('Build React Frontend') {
            steps {
                dir('frontend') {
                    sh '''
                    eval $(minikube docker-env)
                    docker build -t $FRONTEND_IMAGE .
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                eval $(minikube docker-env)

                kubectl delete ingress laravel-ingress --ignore-not-found
                kubectl delete ingress project-ingress --ignore-not-found

                kubectl apply -f k8s/mysql/
                kubectl apply -f k8s/backend/
                kubectl apply -f k8s/frontend/
                kubectl apply -f k8s/ingress/
                '''
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD pipeline completed successfully!'
        }
        failure {
            echo '❌ Build or deployment failed!'
        }
    }
}
