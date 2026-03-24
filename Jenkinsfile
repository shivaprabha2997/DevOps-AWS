pipeline {
    agent any

    environment {
        EMAIL = "akramsyed8046@gmail.com"
        KUBECONFIG_PATH = "/var/lib/jenkins/.kube/config" // Manual path
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

        stage('Test K8s Connection') {
            steps {
                sh """
                echo 'Checking Kubernetes connection...'
                kubectl --kubeconfig=${KUBECONFIG_PATH} get nodes
                """
            }
            post {
                success {
                    emailext subject: "✅ K8s Connection SUCCESS",
                    body: "Kubernetes cluster is reachable.",
                    to: "${EMAIL}"
                }
                failure {
                    emailext subject: "❌ K8s Connection FAILED",
                    body: "Cannot connect to Kubernetes cluster. Check kubeconfig path or permissions.",
                    to: "${EMAIL}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl --kubeconfig=${KUBECONFIG_PATH} apply -f deployment.yaml
                kubectl --kubeconfig=${KUBECONFIG_PATH} apply -f service.yaml
                """
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
                sh """
                kubectl --kubeconfig=${KUBECONFIG_PATH} get pods
                kubectl --kubeconfig=${KUBECONFIG_PATH} get svc
                """
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
