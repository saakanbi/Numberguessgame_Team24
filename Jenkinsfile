pipeline {
    agent any

    tools {
        maven 'Maven'  // Make sure Maven is configured in Jenkins
    }

    environment {
        GIT_REPO = 'https://github.com/saakanbi/Numberguessgame_Team24.git'
        BRANCH = 'main'  
        TOMCAT_URL = 'http://172.31.92.215:8080/manager/text'
        TOMCAT_USER = 'admin'  
        TOMCAT_PASSWORD = 'admin123'  
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${GIT_REPO}"
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
