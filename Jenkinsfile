pipeline {
    agent any

    tools {
        maven 'maven-3'
    }

    environment {
        EC2_IP = "3.15.230.202"
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
    steps {
        sh '''
        scp -o StrictHostKeyChecking=no target/*.war ec2-user@3.15.230.202:/opt/tomcat/webapps/
        '''
    }
}
            
