pipeline {
    agent any

    tools {
        maven 'maven-3'
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
                ls -l target
                scp -o StrictHostKeyChecking=no target/*.war ec2-user@3.15.230.202:/home/ec2-user/
                '''
            }
        }

    }
}
