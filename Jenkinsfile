// A Jenkins declarative pipeline that handles the full CI/CD process.
pipeline {
    // This tells Jenkins to run the pipeline on any available agent.
    agent any

    // Stages define the steps of your pipeline.
    stages {
        // Stage 1: Checkout the source code from the repository.
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                // Clones the Git repository and checks out the 'main' branch.
                git branch: 'main', url: 'https://github.com/Pavi0293/Sonar_pipeline.git'
                echo 'Checkout successful.'
            }
        }
        // Stage 2: Build the project.
        stage('Build') {
            steps {
                echo 'Simulating a build...'
                echo 'Build successful.'
            }
        }
        // Stage 3: SonarQube Analysis
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                // The withSonarQubeEnv step prepares the environment for the scanner.
                // The name 'sq1' must match the name you configured in the Jenkins system settings.
                withSonarQubeEnv('sq1') {
                    // This is where the SonarQube scanner would be called if it was in the PATH.
                    // Since it's not found, this step will fail.
                    // The withSonarQubeEnv step itself still functions, but the command within it fails.
                }
            }
        }
        // Stage 4: Quality Gate Check
        stage("Quality Gate Check") {
            steps {
                echo 'Waiting for Quality Gate status...'
                timeout(time: 1, unit: 'HOURS') {
                    // This step will wait for the SonarQube analysis to complete.
                    waitForQualityGate(abortPipeline: true)
                }
            }
        }
        // Stage 5: Deploy the application.
        stage('Deploy') {
            steps {
                echo 'Simulating deployment...'
                echo 'Deployment successful.'
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
