// Jenkinsfile
pipeline {
    agent any 
    
    tools {
        // This ensures the correct version of Node.js is available
        nodejs 'node-18.17.0' // Replace with your desired Node.js version
    }
    
    stages {
        stage('Checkout') {
            steps {
                // This checks out your code from a Git repository
                git url: 'https://github.com/your-username/your-repo.git', branch: 'main'
            }
        }
        
        stage('Set up NPM Registry') {
            steps {
                // Use the stored Jenkins credentials to create a .npmrc file
                withCredentials([usernamePassword(credentialsId: 'jfrog-npm-credentials', usernameVariable: 'JFROG_USER', passwordVariable: 'JFROG_KEY')]) {
                    sh 'npm config set //arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/:_auth $JFROG_KEY'
                    sh 'npm config set registry https://arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/'
                    sh 'npm config set always-auth true'
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Publish to JFrog') {
            steps {
                sh 'npm publish --access public' // Use --access public for scoped packages
            }
        }
    }
}