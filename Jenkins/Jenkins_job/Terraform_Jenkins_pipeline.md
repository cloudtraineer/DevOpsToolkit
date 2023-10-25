pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your Git repository
                checkout scm
            }
        }
        
        stage('Terraform Init') {
            steps {
                script {
                    sh 'terraform init'
                }
            }
        }
        
        stage('Terraform Plan') {
            steps {
                script {
                    sh 'terraform plan -out=tfplan'
                }
            }
        }
        
        stage('Terraform Apply') {
            steps {
                script {
                    input 'Deploy infrastructure?'
                    sh 'terraform apply tfplan'
                }
            }
        }
    }
    
    post {
        success {
            emailext subject: 'Terraform Apply Successful',
                body: 'Terraform apply was successful',
                to: 'your-email@example.com'
        }
        failure {
            emailext subject: 'Terraform Apply Failed',
                body: 'Terraform apply failed. Please check the Jenkins logs for details.',
                to: 'your-email@example.com'
        }
    }
}
