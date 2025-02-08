pipeline {
    agent any
    environment {
        APP_NAME = "complete-prodcution-e2e-app"
        IMAGE_TAG = "1.0.0-12"
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

        stage('Update the Deployment Tags') {
            steps {
                script {
                    sh '''
                        sed -i 's|image: ankit2849/complete-prodcution-e2e-pipeline:[^ ]*|image: ankit2849/complete-prodcution-e2e-pipeline:${IMAGE_TAG}|g' deployment.yaml
                        cat deployment.yaml
                    '''
                }
            }
        }

        stage('Commit and Push Changes') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-cred', usernameVariable: 'GITHUB_USER', passwordVariable: 'GITHUB_TOKEN')]) {
                        sh '''
                            git config --global user.email "avishwakarma8855@gmail.com"
                            git config --global user.name "Ankit1680"
                            git add deployment.yaml
                            git commit -m "Updated Deployment Manifest with new image tag"
                            git push https://${GITHUB_USER}:${GITHUB_TOKEN}@github.com/Ankit1680/Gitops_argocd main
                        '''
                    }
                }
            }
        }
    }
}
