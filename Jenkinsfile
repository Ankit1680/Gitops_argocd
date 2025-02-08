pipeline{
    agent{
        label "jenkins-agent"
    }

    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
    
    }
    stages{
        stage("Cleanup workspace"){
            steps{
                cleanWs()
            }
           
        }
        stage("Git Checkout"){
            steps{
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/Ankit1680/Gitops_argocd'
            }
           
        }

        
        stage("Update the Deployment Tags") {
            steps {
                sh '''
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                '''
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh '''
                    git config --global user.name "Ankit1680"
                    git config --global user.email "avishwakarma8855@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                '''
                withCredentials([gitUserNamePassword(credentialsId: 'github-cred', gitToolName : 'Default')]) {
                    sh '''
                        git push https://github.com/Ankit1680/Gitops_argocd main
                    '''
                }
            }
        }
           
        
    }
   
}