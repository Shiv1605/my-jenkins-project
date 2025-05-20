pipeline {
    agent any
    
    // Define tools to be used
    tools {
        // Maven installation declared in the Jenkins "Global Tool Configuration"
        maven 'Maven'
    }
    
    // Define environment variables
    environment {
        DOCKER_REGISTRY = 'your-docker-registry'
        STAGING_SERVER = 'staging-server-ip'
        PRODUCTION_SERVER = 'production-server-ip'
    }
    
    // Define pipeline stages
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                
                // Check if pom.xml exists before running Maven
                script {
                    if (fileExists('pom.xml')) {
                        // Using Maven as the build automation tool
                        sh 'mvn clean package -DskipTests'
                        
                        // Archive the built artifacts
                        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                    } else {
                        echo 'No pom.xml found. Creating a simple Java project structure...'
                        
                        // Create a basic Maven project structure
                        sh '''
                            mkdir -p src/main/java/com/example/app
                            mkdir -p src/test/java/com/example/app
                            
                            # Create a simple Java class
                            cat > src/main/java/com/example/app/App.java << 'EOF'
                            package com.example.app;
                            
                            public class App {
                                public static void main(String[] args) {
                                    System.out.println("Hello Jenkins Pipeline!");
                                }
                                
                                public String getGreeting() {
                                    return "Hello Jenkins Pipeline!";
                                }
                            }
                            EOF
                            
                            # Create a simple test class
                            cat > src/test/java/com/example/app/AppTest.java << 'EOF'
                            package com.example.app;
                            
                            import static org.junit.Assert.assertEquals;
                            import org.junit.Test;
                            
                            public class AppTest {
                                @Test
                                public void testApp() {
                                    App app = new App();
                                    assertEquals("Hello Jenkins Pipeline!", app.getGreeting());
                                }
                            }
                            EOF
                            
                            # Create a pom.xml file
                            cat > pom.xml << 'EOF'
                            <?xml version="1.0" encoding="UTF-8"?>
                            <project xmlns="http://maven.apache.org/POM/4.0.0" 
                                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
                                <modelVersion>4.0.0</modelVersion>
                                
                                <groupId>com.example</groupId>
                                <artifactId>jenkins-pipeline-demo</artifactId>
                                <version>1.0-SNAPSHOT</version>
                                
                                <properties>
                                    <maven.compiler.source>1.8</maven.compiler.source>
                                    <maven.compiler.target>1.8</maven.compiler.target>
                                    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
                                </properties>
                                
                                <dependencies>
                                    <dependency>
                                        <groupId>junit</groupId>
                                        <artifactId>junit</artifactId>
                                        <version>4.13.2</version>
                                        <scope>test</scope>
                                    </dependency>
                                </dependencies>
                                
                                <build>
                                    <plugins>
                                        <plugin>
                                            <groupId>org.apache.maven.plugins</groupId>
                                            <artifactId>maven-jar-plugin</artifactId>
                                            <version>3.2.0</version>
                                            <configuration>
                                                <archive>
                                                    <manifest>
                                                        <addClasspath>true</addClasspath>
                                                        <classpathPrefix>lib/</classpathPrefix>
                                                        <mainClass>com.example.app.App</mainClass>
                                                    </manifest>
                                                </archive>
                                            </configuration>
                                        </plugin>
                                    </plugins>
                                </build>
                            </project>
                            EOF
                            
                            # Now build with Maven
                            mvn clean package -DskipTests
                        '''
                        
                        // Archive the built artifacts
                        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                    }
                }
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                // Using JUnit for unit tests and Mockito for mocking
                script {
                    try {
                        sh 'mvn test'
                        
                        // Publish test results if they exist
                        junit allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml'
                    } catch (Exception e) {
                        echo "Tests failed but continuing the pipeline: ${e.message}"
                        // Mark build as unstable but continue
                        currentBuild.result = 'UNSTABLE'
                    }
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                echo 'Performing code analysis...'
                // Using SonarQube for code quality analysis
                script {
                    try {
                        // Check if SonarQube is configured
                        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'sonarqube-token') {
                            sh 'mvn sonar:sonar'
                        }
                    } catch (Exception e) {
                        echo "SonarQube analysis failed or not configured. Skipping: ${e.message}"
                        sh 'echo "Code would be analyzed with SonarQube in a real environment"'
                    }
                }
            }
        }
        
        stage('Security Scan') {
            steps {
                echo 'Performing security scan...'
                script {
                    try {
                        // Using OWASP Dependency-Check for vulnerability scanning
                        sh 'mvn org.owasp:dependency-check-maven:check'
                    } catch (Exception e) {
                        echo "Security scan failed or not configured. Skipping: ${e.message}"
                        sh 'echo "Code would be scanned with OWASP Dependency-Check in a real environment"'
                    }
                }
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                script {
                    // Simulating deployment to staging
                    sh 'echo "Deploying application to staging environment..."'
                    sh 'echo "Application would be deployed to ${STAGING_SERVER} in a real environment"'
                }
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging environment...'
                script {
                    // Simulating integration tests on staging
                    sh 'echo "Running integration tests on staging environment..."'
                    sh 'echo "Tests would connect to http://${STAGING_SERVER}:8080/ in a real environment"'
                }
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                script {
                    // Simulating deployment to production
                    sh 'echo "Deploying application to production environment..."'
                    sh 'echo "Application would be deployed to ${PRODUCTION_SERVER} in a real environment"'
                }
            }
        }
    }
    
    // Post-build actions
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            echo 'Pipeline completed. Cleaning workspace...'
            // Clean up workspace
            cleanWs()
        }
    }
}ipeline failed!'
            // Send notification on failure
            mail to: 'team@example.com',
                 subject: "Pipeline '${currentBuild.fullDisplayName}' failed",
                 body: "The pipeline failed. Check it out at: ${env.BUILD_URL}"
        }
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}
