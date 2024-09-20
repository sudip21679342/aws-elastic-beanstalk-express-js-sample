pipeline {
    agent {
        docker {
            image 'node:16'        // Use Node 16 Docker image
            args '-u root'         // Run as root to avoid permission issues
        }
    }

    environment {
        SNYK_TOKEN = credentials('snyk-org-id')// Store your Snyk token in Jenkins credentials
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
            withCredentials([string(credentialsId: 'snyk-org-id', variable: 'SNYK_TOKEN')]) {
                sh 'echo "${SNYK_TOKEN}" | snyk auth'  // Authenticate with Snyk using the token
            }
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
