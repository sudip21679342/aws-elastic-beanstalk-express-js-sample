pipeline {
    agent {
        docker {
            image 'node:16'        // Use Node 16 Docker image
            args '-u root'         // Run as root to avoid permission issues
        }
    }

    environment {
        SNYK_TOKEN = credentials('snyk-token')  // Store Snyk token in Jenkins credentials
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm install --save'  // Install dependencies using npm
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Running Snyk to scan for vulnerabilities...'
                    sh 'npm install snyk --global'  // Ensure Snyk is installed globally

                    // Authenticate with Snyk using the token directly
                    sh 'snyk config set api=$SNYK_TOKEN'

                    // Run Snyk vulnerability scan
                    sh 'snyk test --severity-threshold=high'  // Scan for vulnerabilities
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    sh 'npm run build'  // Modify as needed if you have a build step
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'npm test'  // Run tests if any
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline complete.'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed due to vulnerabilities or build/test issues!'
        }
    }
}
