pipeline {
    agent any
    environment {
        // This should match the credentials ID in Jenkins
        SONAR_TOKEN = credentials('sonar-token-id')
        DOCKER_IMAGE = 'my-web-app-image'
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies using npm
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run unit tests using npm
                sh 'npm test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis using the specified SonarQube server
                withSonarQubeEnv('MySonarQubeServer') { // Name as configured in Jenkins
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for the SonarQube quality gate check
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Deploy') {
            steps {
                // Example: Use Docker to deploy the application
                sh 'docker run -d -p 3000:3000 $DOCKER_IMAGE'
            }
        }
    }

    post {
        always {
            // Clean workspace after every run
            cleanWs()
        }
    }
}
