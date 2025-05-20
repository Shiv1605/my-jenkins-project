pipeline {
    agent any
    
    stages {
        // Stage 1: Build
        stage('Build') {
            steps {
                echo 'Building the application using Maven'
                sh 'mvn clean package'
            }
        }
        
        // Stage 2: Unit and Integration Tests
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit tests with JUnit and integration tests with TestNG'
                sh 'mvn test'
                sh 'mvn verify -DskipUnitTests=true'
            }
        }
        
        // Stage 3: Code Analysis
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code with SonarQube'
                sh 'mvn sonar:sonar -Dsonar.projectKey=my-project'
            }
        }
        
        // Stage 4: Security Scan
        stage('Security Scan') {
            steps {
                echo 'Scanning for vulnerabilities with OWASP Dependency Check'
                sh 'mvn org.owasp:dependency-check-maven:check'
            }
        }
        
        // Stage 5: Deploy to Staging
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to AWS EC2 staging server'
                sh 'scp target/*.jar user@staging-server:/path/to/deploy'
                sh 'ssh user@staging-server "systemctl restart myapp"'
            }
        }
        
        // Stage 6: Integration Tests on Staging
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment with Selenium'
                sh 'mvn test -Dtest=StagingIntegrationTests'
            }
        }
        
        // Stage 7: Deploy to Production
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to AWS EC2 production server'
                sh 'scp target/*.jar user@production-server:/path/to/deploy'
                sh 'ssh user@production-server "systemctl restart myapp"'
            }
        }
    }
}
