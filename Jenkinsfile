pipeline
{
    agent
    {
        node
        {
            label 'master'
        }
    }
    parameters 
    {
        string description: 'Description of the environment variable', name: 'IMAGE_TAG'
    }
    environment
    {
        IMAGE_TAG = "${params.IMAGE_TAG}"
        FILE_PATH = 'values.yaml'
        BRANCH_NAME = "CBCOPS-1000"
        APP_NAME = "netflix"
    }
    stages
    {
        stage('DevSecOps SCM CheckOut')
        {
            steps
            {
                checkout scmGit(branches: [[name: 'origin/CBCOPS-1000']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/uriyapraba/netflix-CD_pipeline.git']])
            }
        }
        
        stage('Updating helm chart with latest image tag')
        {
            steps
            {
                script
                {
                    sh """
                    echo "Before Update:"
                    cat ${FILE_PATH}
                    sed -i 's/^\\( *tag: \\).*/\\1${IMAGE_TAG}/' ${FILE_PATH}
                    echo "After Update:"
                    cat ${FILE_PATH}
                    """
                }

            }
        }   
        stage('push updated tag to git artifact')
        {
            steps
            {
                    script
                    {
                        sh """
                        git config --global "user.name" "Jenkins-admin"
                        git config --global "user.email" "jenkins@admin.com"
                        git checkout -b ${BRANCH_NAME}
                        git add ${FILE_PATH}
                        git commit -m "Updated new tag"
                        """
                        withCredentials([gitUsernamePassword(credentialsId: 'github_token', gitToolName: 'Default')]) 
                        {
                            sh "git push origin ${BRANCH_NAME}"
                        }
                    }
                
            }
        }
        stage('helm build')
        {
            steps
            {
                script
                {
                    sh """
                    helm upgrade --install . --atomic
                    """
                }
            }
        }
    }
}
