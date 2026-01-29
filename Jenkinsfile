pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
                git branch: "main",
                    url: "https://github.com/ashrafmusalman/python-flask-cicd.git"
            }
        }
        stage("Build and Deploy") {
            steps {
                sh '''
                # Set Docker environment to Minikube AND build in same shell
                eval $(minikube docker-env)
                
                # Build Docker image
                docker build -t flask-app:latest .
                
                # Verify image was built
                docker images | grep flask-app
                
                # Deploy to Kubernetes
                kubectl apply -f k8s/Deployment.yaml
                kubectl apply -f k8s/Service.yaml
                
                # Show deployment status
                kubectl get pods
                kubectl get svc
                '''
            }
        }
    }
    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs above."
        }
        always {
            echo "Pipeline finished."
        }
    }
}
