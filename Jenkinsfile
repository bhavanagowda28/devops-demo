pipeline {
    agent any

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
                scp -o StrictHostKeyChecking=no target/demo.war ec2-user@$EC2_IP:/home/ec2-user/
                ssh ec2-user@$EC2_IP "
                mv demo.war apache-tomcat-9.0.82/webapps/ &&
                cd apache-tomcat-9.0.82/bin &&
                ./shutdown.sh &&
                ./startup.sh
                "
                '''
            }
        }
    }
}
