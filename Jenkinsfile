pipeline {
    agent any
    parameters {
        choice(name: 'ACTION', choices: ['apply', 'destroy'], description: 'Terraform action to run')
    }
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from GitHub"
                git branch: 'main', url: 'https://github.com/rashmigmr13-eng/TERRAFORM-EVENING-BATCH-PROJECT.git'
            }
        }
        stage('Terraform Init') {
            steps {
                echo "Initializing Terraform"
                sh 'terraform init'
            }
        }
        stage('Terraform Validate') {
            steps {
                echo "Validating Terraform configuration"
                sh 'terraform validate'
            }
        }
        stage('Terraform Plan') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                echo "Running Terraform plan"
                sh 'terraform plan -out=tfplan'
            }
        }
        stage('Terraform Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                echo "Applying Terraform configuration"
                sh 'terraform apply -auto-approve tfplan'
            }
        }
        stage('Terraform Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                echo "Destroying Terraform-managed infrastructure"
                sh 'terraform destroy -auto-approve'
            }
        }
    }
    post {
        success {
            echo "Terraform action completed successfully"
        }
        failure {
            echo "Terraform action failed"
        }
    }
}
