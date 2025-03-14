pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs() // Cleans the workspace to remove any residual files
            }
        }
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/HussamQreie/ShopProject.git',
                    branch: 'main',
                    changelog: true,
                    poll: false
            }
        }
        stage('Verify Files') {
            steps {
                sh 'ls -la' // Lists all files to confirm package.json is present
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install' // Installs Node.js dependencies
            }
        }
        stage('Run Tests') {
            steps {
                // Add test commands here (e.g., sh 'npm test')
                echo 'Running tests (to be implemented)'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    // Add SonarQube analysis commands here (e.g., sh 'sonar-scanner')
                    echo 'Performing SonarQube analysis (to be implemented)'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                // Add Quality Gate check logic here
                echo 'Checking Quality Gate (to be implemented)'
            }
        }
        stage('Deploy') {
            steps {
                // Add deployment commands here
                echo 'Deploying application (to be implemented)'
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed. Please check the logs.'
        }
    }
}
