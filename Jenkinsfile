pipeline {
    agent any
    tools {
        // This tool name must match the one configured in Global Tool Configuration
        sonarScanner 'SonarQube Scanner' 
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://your-repository-url.git'
            }
        }
        stage('Build') {
            steps {
                // Your build steps (e.g., `sh 'mvn clean install'`)
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube Server') {
                    // Replace with your project-specific properties
                    sh 'sonar-scanner -Dsonar.projectKey=my-java-project -Dsonar.sources=src'
                }
            }
        }
        stage("Quality Gate Check") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // The `waitForQualityGate` step pauses the pipeline until SonarQube analysis is complete.
                    waitForQualityGate()
                }
            }
        }
    }
    post {
        always {
            // Optional: Add a step to publish the SonarQube report
            echo "SonarQube analysis complete."
        }
    }
}
