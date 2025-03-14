pipeline {
    agent any

    tools {
        maven 'Maven'  // Make sure Maven is configured in Jenkins
    }

    environment {
        PROJECT_NAME = "NumberGuessGame"
        ARTIFACT = "target/NumberGuessGame-1.0-SNAPSHOT.war"
        DEPLOY_DIR = "/home/ec2-user/apache-tomcat-7.0.94/webapps"
        TOMCAT_USER = "ec2-user"
        SERVER_IP = "ec2-user@3.149.248.77" // 🔥 Replace with your Tomcat Server IP
        
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                 git branch: 'main', url: 'https://github.com/saakanbi/Numberguessgame_Team24.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = "target/NumberGuessGame-1.0-SNAPSHOT.war"
                    def tomcatDeployUrl = "${TOMCAT_URL}/deploy?path=/NumberGuessGame"

                    withCredentials([usernamePassword(credentialsId: 'tomcat-credentials', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                        sh """
                        curl --upload-file ${warFile} --user $TOMCAT_USER:$TOMCAT_PASSWORD ${tomcatDeployUrl}
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}
