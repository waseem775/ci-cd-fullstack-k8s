pipeline {
    agent any

    environment {
        BACKEND_IMAGE = "laravel-backend:latest"
        FRONTEND_IMAGE = "react-frontend:latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_USERNAME/ci-cd-fullstack.git'
            }
        }

        stage('Build Laravel Backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t $BACKEND_IMAGE .'
                }
            }
        }

        stage('Run Laravel Tests') {
            steps {
                dir('backend') {
                    sh '''
                        docker run --rm $BACKEND_IMAGE php artisan test
                    '''
                }
            }
        }

        stage('Build React Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build -t $FRONTEND_IMAGE .'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
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
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Build or deployment failed!'
        }
    }
}
