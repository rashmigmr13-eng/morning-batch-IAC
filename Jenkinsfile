pipeline {
    agent any
    parameters {
        choice(name:'ACTION',choices:['apply','destroy'],description: 'Terraform action to run')
    }
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
    stages {
        stage ('CHECKOUT GIT CODE') {
            steps {
                echo "checking out code from github"
                git branch : 'main',url:'https://github.com/rashmigmr13-eng/morning-batch-IAC.git'
            }
        }
        stage('terraform init') {
            steps {
                echo "initializing terraform"
                sh 'terraform init'
            }
        }
        stage('terraform validate') {
            steps {
                echo "validating terraform configuration"
                sh 'terraform validate'
            }
        }
        stage('terraform plan') {
            when {
                expression {params.ACTION == 'apply'}
            }
            steps {
                echo "running terraform plan"
                sh 'terraform plan -out=tfplan'
            }
        }
        stage('terraform apply') {
            when {
                expression {params.ACTION == 'apply'}
            }
            steps {
                echo "applying terraform configuration"
                sh 'terraform apply -auto-approve tfplan'
            }
        }
        stage('terraform destroy') {
            when {
                expression {params.ACTION == 'destroy'}
            }
            steps {
                echo "destroying terraform managed infrasture"
                sh 'terraform destroy -auto-approve'
            }
        }
    }
    post {
        success {
            echo "terraform action completed successfully"
        }
        failure {
            echo "terraform failed"
        }
    }
}
