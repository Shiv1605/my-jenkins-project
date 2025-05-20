pipeline {
    agent any
    
    triggers {
        pollSCM('H/5 * * * *') // Checks GitHub every 5 minutes
    }
    
    tools {
        maven 'Maven_3.9.9' // Pre-configured in Jenkins
        jdk 'JDK_11' 
    }
    
    stages {
        // Stage 1: Build
        stage('Build') {
            steps {
                echo 'Compiling source code with Maven'
                sh 'mvn clean package'
            }
        }
        
        // Stage 2: Unit and Integration Tests
        stage('Tests') {
            steps {
                echo 'Running JUnit tests'
                sh 'mvn test'
                
                echo 'Running TestNG integration tests'
                sh 'mvn verify -DskipUnitTests=true'
            }
            
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' // Publish test reports
                }
            }
        }
        
        // Stage 3: Code Analysis
        stage('Code Analysis') {
            steps {
                echo 'Running SonarQube scan'
                withSonarQubeEnv('SonarQube-Server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        // Stage 4: Security Scan
        stage('Security Scan') {
            steps {
                echo 'Checking vulnerabilities with OWASP'
                sh 'mvn org.owasp:dependency-check:maven'
            }
        }
        
        // Stage 5: Deploy to Staging
        stage('Deploy Staging') {
            steps {
                echo 'Deploying to AWS EC2 staging'
                sh 'aws deploy create-deployment --application-name MyApp --deployment-group-name Staging --s3-location bucket=my-bucket,key=app.war,bundleType=zip'
            }
        }
        
        // Stage 6: Staging Tests
        stage('Staging Tests') {
            steps {
                echo 'Running Selenium tests on staging'
                sh 'mvn test -Pselenium-staging'
            }
        }
        
        // Stage 7: Deploy Production
        stage('Deploy Production') {
            steps {
                echo 'Releasing to AWS EC2 production'
                sh 'aws deploy create-deployment --application-name MyApp --deployment-group-name Production --s3-location bucket=my-bucket,key=app.war,bundleType=zip'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed. Cleaning workspace...'
            cleanWs()
        }
    }
}
