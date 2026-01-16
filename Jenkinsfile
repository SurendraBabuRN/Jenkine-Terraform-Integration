pipeline {
    agent any

    environment {
        AZURE_CREDS = credentials('AZURE_CREDENTIALS')  // Single secret JSON
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/SurendraBabuRN/Jenkine-Terraform-Integration.git'
            }
        }

        stage('Terraform Init') {
            steps {
                bat 'echo %AZURE_CREDS% > azure-creds.json'

                script {
                    def json = readJSON file: 'azure-creds.json'
                    env.ARM_CLIENT_ID       = json.client_id
                    env.ARM_CLIENT_SECRET   = json.client_secret
                    env.ARM_TENANT_ID       = json.tenant_id
                    env.ARM_SUBSCRIPTION_ID = json.subscription_id
                }

                bat 'terraform init -upgrade'
            }
        }

        stage('Terraform Plan') {
            steps {
                bat 'terraform plan'
            }
        }

        stage('Terraform Apply') {
            steps {
                bat 'terraform apply -auto-approve'
            }
        }
    }
}
