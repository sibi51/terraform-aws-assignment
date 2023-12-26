pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                cleanWs()
                git credentialsId: 'github-token', url: "https://github.com/sibi51/terraform-aws-assignment.git", branch: 'jenkins-ci'
                sh "git clone https://github.com/sibi51/terraform-aws-assignment.git"
            }
        }
        stage('AWS Authentication') {
            steps {
               withCredentials([
                  string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),
                  string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')
                  ]) {
                      sh """
                        export AWS_ACCESS_KEY_ID='${AWS_ACCESS_KEY_ID}'
                        export AWS_SECRET_ACCESS_KEY='${AWS_SECRET_ACCESS_KEY}'
                        export AWS_DEFAULT_REGION=eu-west-1
                      """
                  }
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
