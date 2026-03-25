pipeline {
    agent any

    environment {
        EMAIL = "akramsyed8046@gmail.com"
        KUBECONFIG_PATH = "/var/lib/jenkins/.kube/config"
    }

    stages {

        stage('Clone Repository') {
            steps {
                script {
                    try {
                        git branch: 'main',
                            url: 'https://github.com/akramsyed8046/Devops-html-app.git'

                        emailext to: "${EMAIL}",
                        subject: "SUCCESS: Clone Repository",
                        body: "Repository cloned successfully ✅"
                    } catch (e) {
                        emailext to: "${EMAIL}",
                        subject: "FAILED: Clone Repository",
                        body: "Repository clone failed ❌"
                        throw e
                    }
                }
            }
        }

        stage('Test K8s Connection') {
            steps {
                script {
                    try {
                        sh '''
                        kubectl --kubeconfig=$KUBECONFIG_PATH get nodes
                        '''

                        emailext to: "${EMAIL}",
                        subject: "SUCCESS: K8s Connection",
                        body: "Kubernetes connection successful ✅"
                    } catch (e) {
                        emailext to: "${EMAIL}",
                        subject: "FAILED: K8s Connection",
                        body: "Kubernetes connection failed ❌"
                        throw e
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    try {
                        sh '''
                        kubectl --kubeconfig=$KUBECONFIG_PATH apply -f deployment.yaml
                        kubectl --kubeconfig=$KUBECONFIG_PATH apply -f service.yaml
                        '''

                        emailext to: "${EMAIL}",
                        subject: "SUCCESS: Deployment",
                        body: "Application deployed successfully 🚀"
                    } catch (e) {
                        emailext to: "${EMAIL}",
                        subject: "FAILED: Deployment",
                        body: "Deployment failed ❌"
                        throw e
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    try {
                        sh '''
                        kubectl --kubeconfig=$KUBECONFIG_PATH get pods
                        kubectl --kubeconfig=$KUBECONFIG_PATH get svc
                        '''

                        emailext to: "${EMAIL}",
                        subject: "SUCCESS: Verification",
                        body: "Deployment verified successfully ✅"
                    } catch (e) {
                        emailext to: "${EMAIL}",
                        subject: "FAILED: Verification",
                        body: "Verification failed ❌"
                        throw e
                    }
                }
            }
        }
    }
}
