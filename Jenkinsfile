pipeline {

    agent any

    stages {

        stage("Checkout") {
            steps {
                git branch: "main",
                    url: "https://github.com/ashrafmusalman/python-flask-cicd.git"
            }
        }

        stage("Set Docker Env for Minikube") {
            steps {
                sh "eval $(minikube docker-env)"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh "docker build -t flask-app:latest ."
            }
        }

        stage("Deploy to Kubernetes") {
            steps {
                sh """
                kubectl apply -f k8s/Deployment.yaml
                kubectl apply -f k8s/Service.yaml
                """
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
