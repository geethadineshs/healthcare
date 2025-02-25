pipeline {
    agent any

    environment {
        KUBE_CONFIG = credentials('KUBE_CONFIG')  // Kubernetes config stored in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'git-k8s-token', url: 'https://github.com/geethadineshs/healthcare.git', branch: 'master'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'KUBE_CONFIG']) {
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                        sh 'kubectl get nodes'
                        sh 'kubectl get pods'
                        sh 'kubectl get svc'
                    }
                }
            }
        }

        
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed. Check logs!'
        }
    }
}
