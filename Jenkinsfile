pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git branch: 'main', url: 'https://github.com/root-df/aws-loadbalancer-terraform.git'
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
                sh 'terraform plan -out=tfplan.txt'
                sh 'terraform plan -no-color > tfplan-review.txt'
                sh 'cat tfplan-review.txt'
            }
        }
        stage('Review of TF Plan - Approval before TF Apply') {
            steps {
                // Prompt for approval before the build starts
                input(message: 'Review the Terraform plan before build. Do you approve?', submitter: 'user')
            }
        }
        stage('Terraform Apply') {
            steps {
                // Apply the Terraform plan
                sh 'terraform apply -auto-approve tfplan.txt'
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
