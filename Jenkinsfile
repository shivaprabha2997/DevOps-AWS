pipeline {
    agent any

    environment {
        EMAIL = "akramsyed8046@gmail.com"
        KUBECONFIG_PATH = "/var/lib/jenkins/.kube/config"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/akramsyed8046/Devops-html-app.git'
            }
        }

        stage('Test K8s Connection') {
            steps {
                sh '''
                echo "Checking Kubernetes connection..."
                kubectl --kubeconfig=$KUBECONFIG_PATH get nodes
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                echo "Deploying application..."

                # IMPORTANT: using workspace files
                kubectl --kubeconfig=$KUBECONFIG_PATH apply -f deployment.yaml
                kubectl --kubeconfig=$KUBECONFIG_PATH apply -f service.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                kubectl --kubeconfig=$KUBECONFIG_PATH get pods
                kubectl --kubeconfig=$KUBECONFIG_PATH get svc
                '''
            }
        }
    }
}
