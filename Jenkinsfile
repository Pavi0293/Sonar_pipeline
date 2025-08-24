// A Jenkins declarative pipeline that handles the full CI/CD process.
pipeline {
    // This tells Jenkins to run the pipeline on any available agent.
    agent any

    // This section defines the tools available to your pipeline.
    // The older version of the plugin uses 'hudson.plugins.sonar.SonarRunnerInstallation'
    // instead of 'sonarScanner'.
    tools {
        // This tool name must match the one configured in Global Tool Configuration
        // (e.g., if you named the SonarQube Scanner "SonarScanner")
        maven 'Maven 3.8.1' // Example Maven tool
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
                echo 'Building the project with Maven...'
                // Use the configured Maven tool to clean and package the project.
                // This will create the JAR file.
                sh 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                // The withSonarQubeEnv step prepares the environment for the scanner.
                // The name 'sq1' must match the name you configured in the Jenkins system settings.
                // We use the `sonar:sonar` goal for Maven to perform the analysis.
                // The properties are passed as -D flags.
                withSonarQubeEnv('sq1') {
                    sh "mvn sonar:sonar -Dsonar.projectKey=my-java-project -Dsonar.sources=src"
                }
            }
        }
        stage("Quality Gate Check") {
            steps {
                echo 'Waiting for Quality Gate status...'
                timeout(time: 1, unit: 'HOURS') {
                    // The waitForQualityGate() step now takes the required 'abortPipeline' parameter.
                    // Setting it to true will cause the pipeline to fail if the quality gate is not 'OK'.
                    waitForQualityGate(abortPipeline: true)
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
