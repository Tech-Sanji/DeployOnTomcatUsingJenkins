
pipeline {
    agent any
    
    tools {
        jdk 'JDK11'
        maven 'MVN3'
    }
    
    environment {
        // Define environment variables for Tomcat
        WAR_FILE = 'target/Exp5.war' // Path to the generated WAR file (use forward slashes)
        TOMCAT_URL = 'http://localhost:6060' // Tomcat server URL
        TOMCAT_USER = 'rumaan' // Tomcat Manager username
        TOMCAT_PASSWORD = 'shaikh' // Tomcat Manager password
    }
    
    stages {
        stage('Clean Project') {
            steps {
                bat "mvn clean"
            }
        }

        stage('Build Project') {
            steps {
                bat "mvn package"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
		    //in this case it will be C:\ProgramData\Jenkins\.jenkins\workspace\war-deploy-jenkins-tomcat
                    def warFilePath = "${WORKSPACE}/${WAR_FILE}" // Use forward slashes in path
                    echo "WAR file path: ${warFilePath}"
                    
                    // Check if the WAR file exists before deploying
                    if (fileExists(warFilePath)) {
                        echo 'WAR file found, proceeding with deployment...'
                        
                        // Deploy the WAR file to Tomcat using curl and Tomcat Manager API
                        bat """
                            curl --upload-file "${warFilePath}" \
                            --user ${TOMCAT_USER}:${TOMCAT_PASSWORD} \
                            "${TOMCAT_URL}/manager/text/deploy?path=/Exp5&update=true"
                        """
                    } else {
                        error('WAR file not found! Build might have failed.')
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Build completed'
        }
    }
}
