// Jenkinsfile (Corrected)
pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    environment {
        // This ID must exactly match the ID in your Jenkins credentials
        ARTIFACTORY_CREDENTIALS_ID = 'JF_ACCESS_TOKEN'
        NPM_REPO = 'demo-npm'
        GIT_URL = 'https://github.com/barunita/demo-game.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: env.GIT_URL
            }
        }
        
        stage('Set up NPM Registry') {
            steps {
                // Use the corrected credentials ID
                withCredentials([string(credentialsId: env.ARTIFACTORY_CREDENTIALS_ID, variable: 'JFROG_TOKEN')]) {
                    sh 'npm config set //arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/:_auth=$JFROG_TOKEN'
                    sh 'npm config set registry https://arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/'
                    sh 'npm config set always-auth true'
                }
            }
        }
        
        stage('npm Install') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Publish to Artifactory') {
            steps {
                sh 'npm publish'
            }
        }
    }
}
