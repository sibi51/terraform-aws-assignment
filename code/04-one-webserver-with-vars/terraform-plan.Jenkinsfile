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
        stage('Terraform format') {
            steps {
                sh """
                    cd code/04-one-webserver-with-vars
                    terraform fmt -list=true -write=false -diff=true -check=true
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
    }
}
