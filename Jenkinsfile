// Jenkinsfile
pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    environment {
        ARTIFACTORY_CREDENTIALS_ID = 'JF_ACCESS_TOKEN'
        GIT_URL = 'https://github.com/barunita/demo-game.git'
        NPM_REGISTRY_URL = 'https://arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: env.GIT_URL
            }
        }

        stage('Set up NPM Registry') {
            steps {
                // Use the Secret text credential from Jenkins
                withCredentials([string(credentialsId: env.ARTIFACTORY_CREDENTIALS_ID, variable: 'JFROG_TOKEN')]) {
                    sh 'npm config set registry ' + env.NPM_REGISTRY_URL
                    sh 'npm config set ' + env.NPM_REGISTRY_URL.replace('https://', '//') + ':_auth=' + env.JFROG_TOKEN
                    sh 'npm config set always-auth true'
                }
            }
        }

        stage('npm Install') {
            steps {
                // This command will now use your JFrog registry for caching.
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
