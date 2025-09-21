// Jenkinsfile
pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    environment {
        // Your local repository where dependencies will be cached
        NPM_LOCAL_REPO = 'demo-npm-local'
        ARTIFACTORY_CREDENTIALS_ID = 'JF_ACCESS_TOKEN'
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
                withCredentials([string(credentialsId: env.ARTIFACTORY_CREDENTIALS_ID, variable: 'JFROG_TOKEN')]) {
                    sh 'npm config set registry https://arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/'
                    sh 'npm config set //arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/:_auth=$JFROG_TOKEN'
                    sh 'npm config set always-auth true'
                }
            }
        }
        
        stage('npm Install') {
            steps {
                // This command will install your dependencies from public npm
                // and cache them in your JFrog remote repository.
                sh 'npm install'
            }
        }
        
        stage('npm Publish') {
            steps {
                // This command publishes your package to your local repository.
                sh 'npm publish'
            }
        }
    }
}
