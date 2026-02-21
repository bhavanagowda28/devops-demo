pipeline {
    agent any

    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "3.15.230.202"
        TOMCAT_PATH = "/home/ec2-user/apache-tomcat-9.0.82"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/yourusername/devops-demo.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh """
                scp target/demo.war $EC2_USER@$EC2_HOST:$TOMCAT_PATH/webapps/
                ssh $EC2_USER@$EC2_HOST '
                    cd $TOMCAT_PATH/bin
                    ./shutdown.sh
                    ./startup.sh
                '
                """
            }
        }
    }
}


