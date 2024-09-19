pipeline {
    agent any
    tools {
        // This should match the NodeJS installation name in Jenkins (configured via Global Tool Configuration)
        nodejs "Nodejs 20.x"
    }
    environment {
        NODE_ENV = 'production'
        ENV_FILE = credentials('cbs-backend') // replace with your credential ID
    }
    stages {
        stage('Prepare Environment') {
            steps {
                script {
                    // Write the contents of the credential to a .env file
                    writeFile file: '.env', text: "${ENV_FILE}"
                }
            }
        }
        stage('Clone repository') {
            steps {
                // Cloning the repository from GitHub
                git url: 'https://github.com/XyvinTech/cbs-counseling-backend.git', branch: 'main'
            }
        }
        stage('Install dependencies') {
            steps {
                // Running npm install to install dependencies (Windows uses 'bat' instead of 'sh')
                bat 'npm install'
            }
        }
        stage('Start application') {
            steps {
                // Start the application
                bat 'npm run dev'
            }
        }
    }
    post {
        always {
            // Clean up the workspace after the build
            cleanWs()
        }
        failure {
            // Notify if the build fails
            echo "Build failed!"
        }
        success {
            // Notify if the build succeeds
            echo "Build succeeded!"
        }
    }
}
