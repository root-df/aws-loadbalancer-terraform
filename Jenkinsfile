pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git branch: 'main', url: 'https://github.com/root-df/app-in-jenkins.git'
            }
        }
        stage('Terraform Init') {
            steps {
                // Initialize Terraform
                sh 'terraform init'
            }
        }
        stage('Terraform Plan') {
            steps {
                // Generate and save the Terraform plan
                sh 'terraform plan -out=tfplan'
            }
        }
        stage('Terraform Apply') {
            steps {
                // Apply the Terraform plan
                sh 'terraform apply -auto-approve tfplan'
            }
        }
        stage('Terraform Destroy') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('FAILURE') }
            }
            steps {
                // Destroy resources if the build fails.
                sh 'terraform destroy -auto-approve'
            }
        }
    }
}
