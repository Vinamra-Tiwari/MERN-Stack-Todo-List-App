pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('client') {
                    // Set legacy OpenSSL provider for older react-scripts
                    sh 'export NODE_OPTIONS=--openssl-legacy-provider && npm install'
                }
            }
        }
        
        stage('Docker Build Test') {
            steps {
                // Verify that the docker-compose stack builds successfully
                sh 'docker-compose build'
            }
        }
    }
}
