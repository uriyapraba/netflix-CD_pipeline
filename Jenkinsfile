pipeline
{
    agent
    {
        node
        {
            label 'helm'
        }
    }
    stages
    {
        stage('DevSecOps SCM CheckOut')
        {
            steps
            {
                checkout scmGit(branches: [[name: 'origin/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/uriyapraba/DevSecOps-Project.git']])
            }
        }
    }
}