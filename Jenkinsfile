pipeline {
    agent any

    tools {
        maven 'maven-3'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/bhavanagowda28/devops-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['ec2-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no target/demo.war ec2-user@3.15.230.202:/home/ec2-user/
                    '''
                }
            }
        }
    }
}
