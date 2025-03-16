pipeline {
    agent any
    
    tools {
        maven 'Maven 3.8.6'
        jdk 'JDK 11'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
                junit allowEmptyResults: true, testResults: '/target/surefire-reports/*.xml'
            }
        }
        
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }
        
        stage('Deploy to Test') {
            steps {
                // Copy WAR file to EC2 test server with CORRECT filename
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'keyfile')]) {
                    sh """
                        scp -i \$keyfile -o StrictHostKeyChecking=no target/NumberGuessGame-1.0-SNAPSHOT.war ec2-user@18.206.54.243:/tmp/
                        ssh -i \$keyfile -o StrictHostKeyChecking=no ec2-user@18.206.54.243 'sudo cp /tmp/NumberGuessGame-1.0-SNAPSHOT.war ~/apache-tomcat-7.0.94/webapps/NumberGuessGame.war && sudo ~/apache-tomcat-7.0.94/bin/shutdown.sh && sleep 5 && sudo ~/apache-tomcat-7.0.94/bin/startup.sh'
                    """
                }
            }
        }
        
        stage('Deploy to Production') {
            steps {
                // Copy WAR file to EC2 production server with CORRECT filename
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'keyfile')]) {
                    sh """
                        scp -i \$keyfile -o StrictHostKeyChecking=no target/NumberGuessGame-1.0-SNAPSHOT.war ec2-user@18.206.54.243:/tmp/
                        ssh -i \$keyfile -o StrictHostKeyChecking=no ec2-user@18.206.54.243 'sudo cp /tmp/NumberGuessGame-1.0-SNAPSHOT.war ~/apache-tomcat-7.0.94/webapps/NumberGuessGame.war && sudo ~/apache-tomcat-7.0.94/bin/shutdown.sh && sleep 5 && sudo ~/apache-tomcat-7.0.94/bin/startup.sh'
                    """
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            cleanWs()
        }
    }
}
