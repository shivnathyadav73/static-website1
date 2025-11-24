pipeline {
    agent any

    environment {
        KUBECONFIG = "C:\\ProgramData\\Jenkins\\.kube\\config"
        PATH = "C:\\Program Files\\Kubernetes\\;${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling website code...'
                git branch: 'main', url: 'https://github.com/shivnathyadav73/static-website1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                powershell """
                    docker build -t static-website:latest .
                """
            }
        }

        stage('Run Container') {
            steps {
                powershell """
                    docker stop static-web 2>\$null
                    docker rm static-web 2>\$null
                    docker run -d --name static-web -p 9090:80 static-website:latest
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                powershell """
                    echo "Checking Kubernetes cluster access..."
                    kubectl get nodes

                    echo "Deploying to Minikube..."
                    kubectl apply --validate=false -f "${WORKSPACE}\\k8s.yaml"
                """
            }
        }
    }

    post {
        success {
            echo "Website running at: http://localhost:9090"
            echo "Kubernetes deployment applied successfully."
        }
    }
}
