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

        stage('Push Docker Image'){
            steps{
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerpwd')]) {
    
                        sh "docker login -u rummansadhit -p ${dockerpwd}"
                        sh "docker push rummansadhit/nodetest:${Docker_tag}"
                    
                }
            }
        }






    }
    
}


def getDockerTag(){

        def tag = sh script: 'git rev-parse HEAD', returnStdout: true
        return tag
}