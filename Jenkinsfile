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
                sh 'ls -la' // Lists files in the workspace root
                dir('ShopProject-main') {
                    sh 'ls -la' // Lists files inside the ShopProject-main directory
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                dir('ShopProject-main') {
                    sh 'npm install' // Runs npm install inside the repository directory
                }
            }
        }
        stage('Run Tests') {
            steps {
                dir('ShopProject-main') {
                    // Add test commands here (e.g., sh 'npm test')
                    echo 'Running tests (to be implemented)'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    dir('ShopProject-main') {
                        // Add SonarQube analysis commands here (e.g., sh 'sonar-scanner')
                        echo 'Performing SonarQube analysis (to be implemented)'
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                dir('ShopProject-main') {
                    // Add Quality Gate check logic here
                    echo 'Checking Quality Gate (to be implemented)'
                }
            }
        }
        stage('Deploy') {
            steps {
                dir('ShopProject-main') {
                    // Add deployment commands here
                    echo 'Deploying application (to be implemented)'
                }
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
