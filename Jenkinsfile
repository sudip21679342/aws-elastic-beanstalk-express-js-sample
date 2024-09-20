pipeline {
    agent {
        docker {
            image 'node:16'        // Use Node 16 Docker image
            args '-u root --privileged'  // Add --privileged if required for Docker permissions
        }
    }

    environment {
        SNYK_TOKEN = credentials('c5eaa975-48ca-4ed4-aab3-3a6d3e610669') // Ensure this is the API token
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm install --save'  // Install dependencies using npm
                    sh 'npm install snyk --global'  // Install Snyk globally
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Running Snyk to scan for vulnerabilities...'
                    sh 'snyk auth $SNYK_TOKEN'  // Authenticate with Snyk using the token
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
