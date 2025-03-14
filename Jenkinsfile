pipeline {
    agent any
    environment {
        // This should match the credentials ID in Jenkins
        SONAR_TOKEN = credentials('sonar-token-id')
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the correct subfolder
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQubeServer') { // Name as configured in Jenkins
                    sh 'sonar-scanner'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy') {
            steps {
                // Adjust deployment as needed; for example, using Docker to run your container on port 3000:
                sh 'docker run -d -p 3000:3000 my-web-app-image'
            }
        }
    }
}
