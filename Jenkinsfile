pipeline {
    agent any
    environment {
        SONARQUBE_URL = 'http://localhost:9000'  // Update with your SonarQube server URL
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                sh 'ls -la'  // Debugging step to verify the files
            }
        }
        stage('Install Dependencies') {
            steps {
                dir('ShopProject-main') {  // Navigate into the correct directory
                    sh 'npm install'
                }
            }
        }
        stage('Lint Code') {
            steps {
                dir('ShopProject-main') {
                    sh 'npm run lint'  // Make sure linting script exists in package.json
                }
            }
        }
        stage('Run Tests') {
            steps {
                dir('ShopProject-main') {
                    sh 'npm test'  // Ensure you have test scripts in package.json
                }
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONARQUBE_TOKEN = credentials('sonarqube-token')  // Replace with Jenkins credentials ID
            }
            steps {
                dir('ShopProject-main') {
                    sh '''
                    npx sonar-scanner \
                        -Dsonar.projectKey=ShopProject \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONARQUBE_URL \
                        -Dsonar.login=$SONARQUBE_TOKEN
                    '''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Quality gate failed: ${qg.status}"
                        }
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                dir('ShopProject-main') {
                    sh 'docker build -t shopproject .'
                }
            }
        }
        stage('Deploy') {
            steps {
                dir('ShopProject-main') {
                    sh 'docker run -d -p 3000:3000 shopproject'
                }
            }
        }
    }
    post {
        failure {
            echo "Pipeline failed!"
        }
        success {
            echo "Pipeline executed successfully!"
        }
    }
}
