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
                // This command lists the contents of the current directory.
                sh 'ls -lrt'
            }
        }
        // Stage 2: Build the project. Replaced with simple shell commands.
        stage('Build') {
            steps {
                echo 'Simulating a build...'
                // This command lists the contents of the current directory.
                sh 'ls -l'
            }
        }
        // Stage 3: SonarQube Analysis
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                // Add the SonarQube Scanner bin directory to the PATH.
                // You may need to change the path if you installed SonarScanner somewhere else.
                withEnv(['PATH+SONAR=/opt/sonarqube/bin']) {
                    withSonarQubeEnv('sq1') {
                        // We use the direct sonar-scanner command here.
                        // Replace with your project-specific properties.
                        sh 'sonar-scanner -Dsonar.projectKey=my-java-project -Dsonar.sources=src'
                    }
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
        // Stage 5: Deploy the application. Replaced with a simple shell command.
        stage('Deploy') {
            steps {
                echo 'Simulating deployment...'
                // This command will print the contents of a file.
                sh 'cat Jenkinsfile'
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
