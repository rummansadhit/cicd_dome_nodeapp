pipeline{
    agent any 

    environment{

        Docker_tag = getDockerTag()
    }

    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t rummansadhit/nodetest:${Docker_tag}"
            }
        }

    }
    
}


def getDockerTag(){

        def tag = sh script: 'git rev-parse HEAD', returnStdout: true
        return tag
}