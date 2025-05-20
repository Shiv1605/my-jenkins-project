pipeline {
    agent any
    
    tools {
        maven 'Maven'  // This should match the Maven tool name configured in Jenkins
        jdk 'JDK'      // Add this line to specify the JDK tool configured in Jenkins
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the application using Maven'
                sh 'mvn --version'
                sh 'mvn clean package'
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests'
                sh 'mvn test'
            }
        }
        
        stage('Code Analysis') {
            steps {
                echo 'Running code analysis'
                // Add your code analysis commands here
            }
        }
        
        stage('Security Scan') {
            steps {
                echo 'Performing security scan'
                // Add your security scan commands here
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment'
                // Add deployment commands here
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging'
                // Add staging integration test commands here
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production'
                // Add production deployment commands here
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed - cleaning up'
            cleanWs()
        }
        failure {
            echo 'Pipeline failed!'
            emailext(
                subject: 'Pipeline Failed: ${currentBuild.fullDisplayName}',
                body: 'The pipeline has failed. Please check the console output for details.',
                to: 'team@example.com'
            )
        }
        success {
            echo 'Pipeline succeeded!'
            emailext(
                subject: 'Pipeline Succeeded: ${currentBuild.fullDisplayName}',
                body: 'The pipeline has completed successfully.',
                to: 'team@example.com'
            )
        }
    }
}
