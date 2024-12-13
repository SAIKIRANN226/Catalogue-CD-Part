pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    options {
        ansiColor('exterm')
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'HOURS')
    }
    parameters {
        string(name: 'version', defaultValue: '', description: 'What is the artifact version?')
        string(name: 'environment', defaultValue: 'dev', description: 'What is environment?')
    }
    stages {
        stage('Print version') {
            sh """
                echo "version: ${params.version}"
                echo "environment: ${params.environment}"
            """
        }
        stage('Init') {
            sh """
                cd Terraform
                terraform init --backend-config=${params.environment}/backend.tf -reconfigure
            """
        }
        stage('Plan') {
            sh """
                cd Terraform
                terraform plan -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}"
            """
        }
    }
}