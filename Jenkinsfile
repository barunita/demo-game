// Jenkinsfile
pipeline { 
    agent any 

    tools { 
        nodejs 'node' 
        jfrog 'jfrog-cli' 
    } 

    environment { 
        // Corrected the Git URL to your new repository
        GIT_URL = 'https://github.com/barunita/demo-game.git'
        ARTIFACTORY_URL = "https://arunitatrial123.jfrog.io" 
        ARTIFACTORY_CREDENTIALS_ID = 'JF_ACCESS_TOKEN' 
        // Corrected the variable name to reflect your repository
        NPM_REPO = 'demo-npm' 
    } 

    stages { 
        stage('Checkout') { 
            steps { 
                // Using the corrected Git URL from the environment variable
                git branch: 'main', url: env.GIT_URL
            } 
        } 
        
        stage('Configure JFrog CLI') { 
            steps { 
                script { 
                    try { 
                        withCredentials([string(credentialsId: env.ARTIFACTORY_CREDENTIALS_ID, variable: 'ART_TOKEN')]) { 
                            sh "${tool 'jfrog-cli'}/jf c add arunitatrial123 --url=${env.ARTIFACTORY_URL} --access-token=${ART_TOKEN}" 
                        } 
                    } catch (err) { 
                        echo "Warning: JFrog server 'arunitatrial123' already exists. Proceeding without re-adding." 
                    } 
                } 
            } 
        } 
        
        stage('npm Install') { 
            steps { 
                sh 'npm install' 
            } 
        } 
        
        stage('Configure JFrog npm') { 
            steps { 
                // Correctly referencing the NPM_REPO variable
                sh "${tool 'jfrog-cli'}/jf npm-config --repo-resolve ${env.NPM_REPO} --repo-deploy ${env.NPM_REPO} --server-id-deploy arunitatrial123 --server-id-resolve arunitatrial123" 
            } 
        } 
        
        stage('Publish to Artifactory') { 
            steps { 
                // Explicitly defining the deployment repository to ensure publish works
                sh "${tool 'jfrog-cli'}/jf npm publish --repo-deploy ${env.NPM_REPO}" 
            } 
        } 
    } 
}
