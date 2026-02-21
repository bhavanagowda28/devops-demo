pipeline {
    agent any

    stages {

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                scp -o StrictHostKeyChecking=no target/demo.war ec2-user@3.15.230.202:/home/ec2-user/
                ssh ec2-user@3.15.230.202 "mv demo.war apache-tomcat-9.0.82/webapps/ && cd apache-tomcat-9.0.82/bin && ./shutdown.sh && ./startup.sh"
                '''
            }
        }

    }
}


