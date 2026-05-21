pipeline {
    agent any
    
    tools {
        // Tells Jenkins to use the NodeJS installation we will configure
        nodejs 'Node20'
    }

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
                    sh 'export NODE_OPTIONS=--openssl-legacy-provider && npm install'
                }
            }
        }
        
        stage('Dependency Check') {
            steps {
                // Perform a dependency check on the backend and frontend
                // using npm's built-in vulnerability auditing tool
                dir('backend') {
                    sh 'npm audit --audit-level=high || true'
                }
                dir('client') {
                    sh 'npm audit --audit-level=high || true'
                }
            }
        }
        
        stage('Security Check (SonarQube)') {
            environment {
                // Example of integrating SonarQube
                scannerHome = tool 'SonarScanner'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=MERN-App -Dsonar.sources=."
                }
            }
        }
    }
}
