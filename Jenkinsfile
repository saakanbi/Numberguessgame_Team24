pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven'  // Ensure Maven is installed in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/saakanbi/Numberguessgame_Team24.git'
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

        stage('Deploy') {
            steps {
                sh '''
                mv target/NumberGuessGame-1.0-SNAPSHOT.war target/NumberGuessGame.war
                cp target/NumberGuessGame.war /home/ec2-user/apache-tomcat-7.0.94/webapps/
                '''
            }
        }
    } 

    post {
        success {
            echo '✅ Build and Deployment Successful'
        }
        failure {
            echo '❌ Build Failed'
        }
    }
}
