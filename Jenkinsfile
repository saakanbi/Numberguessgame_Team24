pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-repo/NumberGuessGame.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }
        stage('Deploy') {
            steps {
                deployToTomcat()
            }
        }
    }
}

def deployToTomcat() {
    sh '''
    scp target/*.war user@your-server:/path/to/tomcat/webapps/
    ssh user@your-server "systemctl restart tomcat"
    '''
}

