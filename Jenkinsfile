pipeline {
    agent any
    
    tools {
        maven 'Maven' // This should match the Maven installation name in Jenkins
    }
    
    options {
        // Use skipDefaultCheckout if you're manually checking out
        skipDefaultCheckout false
    }
    
    environment {
        // Set any environment variables needed
        JAVA_HOME = '/usr/lib/jvm/default-java'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                         branches: [[name: '*/main']], // Explicitly specify main branch
                         userRemoteConfigs: [[url: 'https://github.com/Shiv1605/my-jenkins-project.git']]])
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
                echo 'Running unit tests with JUnit and integration tests with TestNG'
                sh 'mvn test'
                sh 'mvn verify -DskipUnitTests=true'
            }
        }
        
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code with SonarQube'
                sh 'mvn sonar:sonar -Dsonar.projectKey=my-project'
            }
        }
        
        stage('Security Scan') {
            steps {
                echo 'Scanning for vulnerabilities with OWASP Dependency Check'
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to AWS EC2 staging server'
                script {
                    sh 'scp target/*.jar user@staging-server:/path/to/deploy'
                    sh 'ssh user@staging-server "systemctl restart myapp"'
                }
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment with Selenium'
                script {
                    sh 'mvn test -Dtest=StagingIntegrationTests'
                }
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to AWS EC2 production server'
                script {
                    sh 'scp target/*.jar user@production-server:/path/to/deploy'
                    sh 'ssh user@production-server "systemctl restart myapp"'
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed - cleaning up'
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
            emailext body: 'Pipeline failed!', subject: 'Pipeline Failed', to: 'team@example.com'
        }
    }
}
