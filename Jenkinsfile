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
    }
    stages
    {
        stage('DevSecOps SCM CheckOut')
        {
            steps
            {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/uriyapraba/netflix-CD_pipeline.git']])
            }
        }
        
        stage('Updating helm chart with latest image tag')
        {
            steps
            {
                script
                {
                    sh '''
                    echo "ImageTag: ${IMAGE_TAG}"
                    '''
                }

            }
        }
    }

}