pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('sonar-token-id')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            agent {
                docker { image 'node:16' }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            agent {
                docker { image 'node:16' }
            }
            steps {
                sh 'npm test'
            }
        }
        stage('SonarQube Analysis') {
            agent {
                docker { image 'node:16' }
            }
            steps {
                withSonarQubeEnv('MySonarQubeServer') {
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
                sh 'docker run -d -p 3000:3000 my-web-app-image'
            }
        }
    }
}
