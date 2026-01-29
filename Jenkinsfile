pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-app"
    }

    stages {
        stage("Checkout") {
            steps {
                echo "Checking out code from GitHub..."
                git branch: "main",
                    url: "https://github.com/ashrafmusalman/python-flask-cicd.git"
            }
        }

        stage("Set Docker Env for Minikube") {
            steps {
                echo "Configuring Docker to use Minikube..."
                sh 'eval $(minikube docker-env)'
            }
        }

        stage("Build Docker Image") {
            steps {
                echo "Building Docker image with latest code..."
                // --no-cache ensures fresh build every time
                sh 'docker build --no-cache -t $IMAGE_NAME:latest .'
            }
        }

        stage("Deploy to Kubernetes") {
            steps {
                echo "Applying Kubernetes manifests..."
                sh '''
                kubectl apply -f k8s/Deployment.yaml
                kubectl apply -f k8s/Service.yaml

                echo "Restarting pods to pick up new image..."
                kubectl rollout restart deployment flask-deployment
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
        success {
            echo "✅ Build and deployment completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs for errors."
        }
    }
}
