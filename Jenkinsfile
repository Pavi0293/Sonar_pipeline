// A Jenkins declarative pipeline that handles the full CI/CD process.
pipeline {
    // This tells Jenkins to run the pipeline on any available agent.
    agent any

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

        // Stage 2: Build the application using Maven.
        stage('Build') {
            steps {
                echo 'Building the application with Maven...'
                // The 'dir' step changes the working directory to the project folder
                // that contains the pom.xml file. You MUST replace 'your-project-directory'
                // with the actual name of your project folder.
                dir('your-project-directory') {
                    // The 'clean install' goal compiles, tests, and packages the application.
                    // It assumes Maven is available in the agent's PATH.
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        // Stage 3: Deploy the built artifact to the target server.
        stage('Deploy') {
            steps {
                echo "Deploying application to ${env.DEPLOY_PATH}..."
                // Create the deployment directory if it doesn't exist.
                sh "mkdir -p ${env.DEPLOY_PATH}"
                // Copy the built JAR file to the deployment path.
                sh "cp ${env.JAR_FILE} ${env.DEPLOY_PATH}/"
            }
        }

        // Stage 4: Start the application on the server.
        stage('Start') {
            steps {
                echo 'Starting the application...'
                // This command starts the JAR as a background process using 'nohup'.
                // The '&' symbol detaches it from the terminal so the pipeline can continue.
                sh "nohup java -jar ${env.DEPLOY_PATH}/${env.JAR_FILE} > /dev/null 2>&1 &"
                // Give the app a moment to start up before proceeding.
                sleep 5
            }
        }
        
        // Stage 5: Run a basic test to ensure the application is running.
        stage('Test') {
            steps {
                echo 'Running tests against the deployed application...'
                // You would replace this with a real test, like a cURL command
                // to hit an endpoint of your application and check for a successful response.
                sh 'echo "Simulating a test..."'
                // Example of a real test:
                // sh "curl --fail --silent --show-error http://${env.REMOTE_SERVER}:8080/health"
            }
        }
        
        // Stage 6: Perform the SonarQube analysis.
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                // This step now uses the credentials you created.
                // It passes the server name as the first positional argument and the credentials as a named argument.
                withSonarQubeEnv('jenkins') {
                    // This command executes the SonarQube analysis with Maven,
                    // passing the authentication token as a command-line property.
                    withCredentials([string(credentialsId: 'sonarqube-auth-token', variable: 'SONAR_TOKEN')]) {
                        sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
                    }
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
