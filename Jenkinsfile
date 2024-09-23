// pipeline {
//     agent {
//         docker {
//             image 'node:16'        // Use Node 16 Docker image
//             args '-u root'  // Only use --privileged if absolutely necessary
//         }
//     }

//     environment {
//         SNYK_TOKEN = credentials('c5eaa975-48ca-4ed4-aab3-3a6d3e610669')  // Ensure this is the API token
//     }

//     stages {
//         stage('Install Dependencies') {
//             steps {
//                 script {
//                     echo 'Installing dependencies...'
//                     sh 'npm install'  // Simplified to just npm install
//                     sh 'npm install snyk --global'  // Install Snyk globally
//                 }
//             }
//         }

//         stage('Security Scan') {
//             steps {
//                 script {
//                     echo 'Running Snyk to scan for vulnerabilities...'
//                     sh '''
//                     if [ -n "$SNYK_TOKEN" ]; then
//                         snyk auth $SNYK_TOKEN
//                         snyk test --severity-threshold=high
//                     else
//                         echo "Snyk token is not set!"
//                         exit 1
//                     fi
//                     '''
//                 }
//             }
//         }

//         stage('Build') {
//             steps {
//                 script {
//                     echo 'Building the project...'
//                     sh 'npm run build'  // Ensure you have a build script in package.json
//                 }
//             }
//         }

//         stage('Test') {
//             steps {
//                 script {
//                     echo 'Running tests...'
//                     sh 'npm test'  // Standard npm test command
//                 }
//             }
//         }
//     }

//     post {
//         always {
//             echo 'Pipeline complete.'
//         }
//         success {
//             echo 'Pipeline succeeded!'
//         }
//         failure {
//             script {
//                 echo 'Pipeline failed due to vulnerabilities or build/test issues!'
//                 // Optional: Add a command to fetch more detailed logs if needed
//             }
//         }
//     }
// }


pipeline {
    agent { docker { image 'node:16' } }
    stages {
        stage('Install dependencies') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm install --save'
                sh 'npm install express@4.20.0'
            }
        }
        stage('Snyk Scan') {
            steps {
                echo 'Scanning...'
                sh '''
                npm install snyk -g
                snyk auth 7c5eaa975-48ca-4ed4-aab3-3a6d3e610669
                snyk test --severity-threshold=high
                '''
            }
        }
    }
}