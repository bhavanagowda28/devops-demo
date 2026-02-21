pipeline {
    agent any

    tools {
        maven 'maven-3'
    }

    options {
        timestamps()
    }

    stages {

        stage('Build') {
            steps {
                echo "Starting Maven Build..."
                sh 'mvn --version'
                sh 'mvn clean package'
            }
        }

        stage('Verify WAR File') {
            steps {
                echo "Checking if WAR file exists..."
                sh 'ls -lh target/'
            }
        }

        stage('Deploy to EC2') {
            steps {
                echo "Deploying WAR to EC2..."
                sshagent(['ec2-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no target/demo.war ec2-user@3.15.230.202:/home/ec2-user/
                    '''
                }
            }
        }

        stage('Move WAR to Tomcat') {
            steps {
                echo "Moving WAR into Tomcat webapps..."
                sshagent(['ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@3.15.230.202 "
                    mv /home/ec2-user/demo.war /home/ec2-user/apache-tomcat-9.0.82/webapps/ &&
                    cd /home/ec2-user/apache-tomcat-9.0.82/bin &&
                    ./shutdown.sh &&
                    ./startup.sh
                    "
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Build or Deployment Failed. Check console output carefully."
        }
    }
}
}
