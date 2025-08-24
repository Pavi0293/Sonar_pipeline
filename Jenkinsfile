// A Jenkins declarative pipeline that handles the full CI/CD process.
pipeline {
    // This tells Jenkins to run the pipeline on any available agent.
    agent any
   tools {
        // This tool name must match the one configured in Global Tool Configuration
        // (e.g., if you named the SonarQube Scanner "SonarScanner")
        sonarScanner 'SonarQube Scanner' 
    }
    // Define environment variables that can be used throughout the pipeline.
    environment {
        // Specify the directory where the application will be deployed.
        // You can change this to your desired path.
        DEPLOY_PATH = '/opt/my-app'
        
        // This is a placeholder for the server IP.
        // If your deployment server is different, you'll need to configure SSH access.
        REMOTE_SERVER = 'localhost'
        
        // Name of the JAR file to be deployed.
        // This assumes your Maven build creates a JAR.
        JAR_FILE = 'target/my-app-1.0-SNAPSHOT.jar'
    }

    // Stages define the steps of your pipeline.
    stages {
        // Stage 1: Checkout the source code from the repository.
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                // Clones the Git repository and checks out the 'main' branch.
                git branch: 'main', url: 'https://github.com/Pavi0293/Sonar_pipeline.git'
            }
        }
         stage('Build') {
            steps {
                // Your build steps (e.g., `sh 'mvn clean install'`)
            }
        }
         stage('SonarQube Analysis') {
            steps {
                // The name 'sq1' must match the name you configured in the Jenkins system settings
                withSonarQubeEnv('sq1') { 
                    // Replace with your project-specific properties
                    sh 'sonar-scanner -Dsonar.projectKey=my-java-project -Dsonar.sources=src'
                }
            }
        }
        stage("Quality Gate Check") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // This step will wait for the SonarQube analysis to complete
                    waitForQualityGate()
                }
            }
        }
    }

    // A 'post' section to run actions after the pipeline completes.
    post {
        always {
            echo 'Pipeline finished.'
            // You can add cleanup or notification steps here.
        }
        
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
