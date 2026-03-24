pipeline {
    agent any

    environment {
        EMAIL = "akramsyed8046@gmail.com"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/akramsyed8046/Devops-html-app.git'
            }
            post {
                success {
                    emailext subject: "✅ Clone SUCCESS",
                    body: "Repository cloned successfully.",
                    to: "${EMAIL}"
                }
                failure {
                    emailext subject: "❌ Clone FAILED",
                    body: "Repository cloning failed.",
                    to: "${EMAIL}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh '''
                    export KUBECONFIG=$KUBECONFIG
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                    '''
                }
            }
            post {
                success {
                    emailext subject: "✅ Deployment SUCCESS",
                    body: "Application deployed to Kubernetes successfully.",
                    to: "${EMAIL}"
                }
                failure {
                    emailext subject: "❌ Deployment FAILED",
                    body: "Kubernetes deployment failed. Check logs.",
                    to: "${EMAIL}"
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh '''
                    export KUBECONFIG=$KUBECONFIG
                    kubectl get pods
                    kubectl get svc
                    '''
                }
            }
            post {
                success {
                    emailext subject: "✅ Verification SUCCESS",
                    body: "Pods and services are running successfully.",
                    to: "${EMAIL}"
                }
                failure {
                    emailext subject: "❌ Verification FAILED",
                    body: "Verification failed. Check Kubernetes resources.",
                    to: "${EMAIL}"
                }
            }
        }
    }
}
