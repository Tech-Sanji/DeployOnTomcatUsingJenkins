pipeline { 
    agent any 
 
    tools { 
        jdk 'JDK11' 
        maven 'MAVEN3' 
    } 
 
    environment { 
        WAR_FILE = 'target\\roshambo.war' 
        TOMCAT_URL = 'http://localhost:6060' 
        TOMCAT_USER = 'rumaan' 
        TOMCAT_PASSWORD = 'shaikh' 
    } 
 
    stages { 
        stage('Checkout') { 
            steps { 
                git branch: 'main', url: 
'https://github.com/Tech-Sanji/DeployOnTomcatUsingJenkins.git' 
            } 
        } 
 
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
                    def warFilePath = "${WORKSPACE}\\${WAR_FILE}" 
                    if (fileExists(warFilePath)) { 
                        bat """ 
                            curl --upload-file "${warFilePath}" ^ 
                            --user ${TOMCAT_USER}:${TOMCAT_PASSWORD} ^
"${TOMCAT_URL}/manager/text/deploy?path=/roshambo&update=true" 
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
