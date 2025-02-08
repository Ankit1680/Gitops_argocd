pipeline {
    agent any
    environment {
        APP_NAME = "complete-prodcution-e2e-app"
        IMAGE_TAG = "1.0.0-12"  // Change based on your versioning logic
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
                        cat deployment.yaml
                        
                        sed -i 's|image: ankit2849/complete-prodcution-e2e-pipeline:[^ ]*|image: ankit2849/complete-prodcution-e2e-pipeline:${IMAGE_TAG}|g' deployment.yaml
                        
                        cat deployment.yaml
                    '''
                }
            }
        }
        stage('Commit and Push Changes') {
            steps {
                script {
                    sh '''
                        git add deployment.yaml
                        
                        git commit -m "Updated Deployment Manifest with new image tag"
                        
                        git push https://github.com/Ankit1680/Gitops_argocd main
                    '''
                }
            }
        }
    }
}
















// pipeline {
//     agent {
//         label "jenkins-agent"
//     }

//     environment {
//         APP_NAME = "complete-prodcution-e2e-pipeline"
//         IMAGE_TAG = "${APP_NAME}:${BUILD_NUMBER}"  
//     }

//     stages {
//         stage("Cleanup workspace") {
//             steps {
//                 cleanWs()
//             }
//         }

//         stage("Git Checkout") {
//             steps {
//                 git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/Ankit1680/Gitops_argocd'
//             }
//         }

//         stage("Update the Deployment Tags") {
//             steps {
//                 sh '''
//                     cat deployment.yaml
//                     sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
//                     cat deployment.yaml  
//                     git status
//                     git add -f deployment.yaml
//                     git commit -m "Updated Deployment Manifest"
//                 '''
//             }
//         }

//         stage("Push the changed deployment file to Git") {
//             steps {
//                 withCredentials([gitUserNamePassword(credentialsId: 'github-cred', gitToolName: 'Default')]) {
//                     sh '''
//                          git push https://github.com/Ankit1680/Gitops_argocd main
//                     '''
//                 }
//             }
//         }
//     }
// }
