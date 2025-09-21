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
                // Use a credentials binding and write to a .npmrc file
                withCredentials([string(credentialsId: env.ARTIFACTORY_CREDENTIALS_ID, variable: 'JFROG_TOKEN')]) {
                    sh """
                        echo "registry=${NPM_REGISTRY_URL}" > .npmrc
                        echo "//arunitatrial123.jfrog.io/artifactory/api/npm/demo-npm/:_auth=$JFROG_TOKEN" >> .npmrc
                        echo "always-auth=true" >> .npmrc
                    """
                }
            }
        }

        stage('npm Install') {
            steps {
                // npm will now read the .npmrc file and use it
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
