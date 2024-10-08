pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git url: 'https://github.com/SRIVATSA2002/cicd-end-to-end',
                branch: 'main'
           }
        }

        stage('Build docker image'){
            steps{
                script{
                    sh '''
                    echo 'Build Docker Image'
                    docker build -t srivatsa200/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push srivatsa200/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git url: 'https://github.com/SRIVATSA2002/cicd-demo-manifests-repo.git',
                branch: 'master'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/SRIVATSA2002/cicd-demo-manifests-repo.git HEAD:master
                        '''                        
                }
            }
        }
    }
}
