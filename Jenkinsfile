// Jenkinsfile
pipeline {
    agent any

    tools {
        nodejs 'node'
        jfrog 'jfrog-cli'
    }

    environment {
        // You can define your JFrog credentials and repo name as variables
        ARTIFACTORY_CREDENTIALS_ID = 'jfrog-credentials'
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
                // Use withCredentials to securely access JFrog credentials
                withCredentials([usernamePassword(credentialsId: env.ARTIFACTORY_CREDENTIALS_ID, usernameVariable: 'JFROG_USER', passwordVariable: 'JFROG_KEY')]) {
                    sh 'npm config set //arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/:_auth=$JFROG_KEY'
                    sh 'npm config set registry https://arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/'
                    sh 'npm config set always-auth true'
                }
            }
        }
        
        stage('npm Install') {
            steps {
                // Install dependencies from the configured registry
                sh 'npm install'
            }
        }
        
        stage('Publish to Artifactory') {
            steps {
                // Publish the package using the npm command
                sh 'npm publish'
            }
        }
    }
}
