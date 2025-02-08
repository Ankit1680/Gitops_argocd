pipeline {
    agent {
        label "jenkins-agent"
    }

    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
        IMAGE_TAG = "${APP_NAME}:${BUILD_NUMBER}"  
    }

    stages {
        stage("Cleanup workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Git Checkout") {
            steps {
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/Ankit1680/Gitops_argocd'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh '''
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml  
                    git status
                    git add -f deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                '''
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-cred', gitToolName: 'Default')]) {
                    sh '''
                        git remote set-url origin https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Ankit1680/Gitops_argocd
                        git push origin main
                    '''
                }
            }
        }
    }
}
