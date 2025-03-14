pipeline {
    agent {
        docker {
            image 'node:18-alpine' // Use an official Node.js image
            args '-p 3000:3000' // Map ports if needed
        }
    }
    environment {
        SONAR_TOKEN = credentials('sonar-token') // SonarQube token from Jenkins credentials
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm // Single checkout step
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test' // Ensure tests exist in your project
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Configure SonarQube server in Jenkins
                    sh 'npx sonar-scanner -Dsonar.projectKey=your-project-key'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'npm run build' // Example build step
                // Add deployment steps (e.g., deploy to server/S3)
            }
        }
    }
}
