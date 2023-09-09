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
            }
            post {
                success {
                    // Prompt for approval
                    input(message: 'Review the Terraform plan. Do you approve?', submitter: 'devops')
                }
            }
        }
        stage('Terraform Apply') {
            steps {
                // Apply the Terraform plan
                sh 'terraform applyy -auto-approve tfplan.txt'
            }
        }
        stage('Terraform Destroy') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('FAILURE') }
            }
            steps {
                // Destroy resources if the build fails.
                sh 'terraform destroyy -auto-approve'
            }
        }
    }
}
