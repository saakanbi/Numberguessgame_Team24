pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven'  // Ensure Maven is installed in Jenkins
        TOMCAT_HOME = '/home/ec2-user/apache-tomcat-7.0.94' // Path to Tomcat home directory
        TOMCAT_USER = 'admin' // Tomcat manager username
        TOMCAT_PASSWORD = 'admin123' // Tomcat manager password
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Change the branch name to 'wole' here
                git branch: 'wole', url: 'https://github.com/saakanbi/Numberguessgame_Team24.git'
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
                script {
                    // Define the path to the WAR file
                    def warFile = 'target/NumberGuessGame-1.0-SNAPSHOT.war'
                    def warDest = 'target/NumberGuessGame.war'

                    // Rename the WAR file
                    sh "mv ${warFile} ${warDest}"

                    // Deploy the WAR file to Tomcat's webapps directory
                    echo "Deploying the WAR file to Tomcat..."
                    sh """
                    cp ${warDest} ${TOMCAT_HOME}/webapps/
                    """

                    // Restart Tomcat to deploy the application (if needed)
                    echo "Restarting Tomcat to apply the deployment..."
                    sh """
                    ${TOMCAT_HOME}/bin/shutdown.sh
                    sleep 5
                    ${TOMCAT_HOME}/bin/startup.sh
                    """
                }
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
