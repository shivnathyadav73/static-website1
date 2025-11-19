pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "shivsoftapp"
        DOCKERHUB_PASS = "india@12345"
        IMAGE_NAME = "static-website"
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                echo 'Pulling website code from GitHub...'
                git branch: 'main', url: 'https://github.com/shivnathyadav73/static-website1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image..."
                powershell """
                    docker build -t ${IMAGE_NAME}:latest .
                """
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing image to Docker Hub..."
                powershell """
                    docker tag ${IMAGE_NAME}:latest ${DOCKERHUB_USER}/${IMAGE_NAME}:latest
                    docker login -u ${DOCKERHUB_USER} -p ${DOCKERHUB_PASS}
                    docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to Kubernetes cluster..."
                powershell """
                    kubectl apply -f k8s-deployment.yaml
                    kubectl apply -f k8s-service.yaml
                """
            }
        }
    }

    post {
        success {
            echo "🎉 Website successfully deployed from GitHub to Kubernetes!"
        }
        failure {
            echo "❌ Something went wrong!"
        }
    }
}
