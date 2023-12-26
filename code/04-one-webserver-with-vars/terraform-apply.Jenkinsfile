pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    stages {
        stage('Git Clone') {
            steps {
                cleanWs()
                git credentialsId: 'github-token', url: "https://github.com/sibi51/terraform-aws-assignment.git", branch: 'jenkins-ci'
                sh "git clone https://github.com/sibi51/terraform-aws-assignment.git"
            }
        }
        stage('Terraform init') {
            steps {
                sh """
                    cd code/04-one-webserver-with-vars
                    terraform init
                """
            }
        }
        stage('Terraform plan') {
            steps {
                sh """
                    cd code/04-one-webserver-with-vars
                    terraform plan -var 'server_port=8080'
                """
            }
        }
        stage('Terraform apply') {
            steps {
                sh """
                    cd code/04-one-webserver-with-vars
                    terraform apply -auto-approve -var 'server_port=8080'
                """
            }
        }
    }
}
