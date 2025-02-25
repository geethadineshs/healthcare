pipeline {
    agent any

    environment {
        KUBE_CONFIG = credentials('KUBE_CONFIG')  // Kubernetes config stored in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/geethadinesh/your-repo.git'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'KUBE_CONFIG']) {
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                        sh 'kubectl get pods'
                        sh 'kubectl get svc'
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh 'curl -I http://13.233.131.74:30007/'
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
