pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                echo "Build the code using Maven."
            }
        }
        stage('Unit and Integration Test'){
            steps{
                echo "Running Unit tests to ensure the code functions as expected..."
                echo "Running Integration tests to ensure the different components of the application work together as expected, using Selenium..."
                bat "del test.zip"
                zip zipFile: 'test.zip', archive: false, dir: 'target'
            }
        }
        post {
            failure {
                emailext attachmentsPattern: 'test.zip', body: '''${SCRIPT, template="groovy-html.template"}''', 
                        subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
                        mimeType: 'text/html',to: "email id"
                }
             success {
                   emailext attachmentsPattern: 'test.zip', body: '''${SCRIPT, template="groovy-html.template"}''', 
                        subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                        mimeType: 'text/html',to: "email id"
              }      
        }
        stage('Code Analysis'){
            steps{
                echo "Check the quality of the code, using SonarQube..."
            }
        }
        stage('Security Scan'){
            steps{
                echo "Scanning for security vulnerabilities in code, using Codesonar..."
                bat "del security_scan.zip"
                zip zipFile: 'security_scan.zip', archive: false, dir: 'target'
            }
        }
        post {
            failure {
                emailext attachmentsPattern: 'security_scan.zip', body: '''${SCRIPT, template="groovy-html.template"}''', 
                        subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Failed", 
                        mimeType: 'text/html',to: "email id"
                }
             success {
                   emailext attachmentsPattern: 'security_scan.zip', body: '''${SCRIPT, template="groovy-html.template"}''', 
                        subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                        mimeType: 'text/html',to: "email id"
              }      
        }
        stage('Deploy'){
            steps{
                echo "Deploying the application to a staging server on AWS EC2 instance..."
            }
        }
        stage('Integration tests on Staging'){
            steps{
                echo "Runnning integration tests on the staging environment to ensure the application functions as expected in a production-like environment..."
            }
        }
        stage('Deploy to Production'){
            steps{
                echo "Deploying the application to a production server on AWS EC2 instance..."
            }
        }
    }
}
