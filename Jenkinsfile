pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sujithchandran1/Boutique_App.git'
            }
        }
        stage('Configure AWS') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds']]) {
                    sh '''
                    aws sts get-caller-identity
                    aws eks update-kubeconfig --region ap-northeast-1 --name demo-cluster
                    '''
                    }
                }
            }
        stage('Build & Push Docker Images') {
            steps {
                sh '''
                chmod +x Microservices/docker_image_buid_push.sh
                ./Microservices/docker_image_buid_push.sh
                '''
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                kubectl apply -f Microservices/kubernetes-manifests
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                kubectl get pods
                '''
            }
        }
    }
}