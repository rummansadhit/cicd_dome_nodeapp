pipeline{
    agent any 

    environment{

        Docker_tag = getDockerTag()
    }

    stages{
        stage('Build Docker Image'){
            steps{


            withCredentials([string(credentialsId: 'acr_password', variable: 'acrpwd')]) {

                sh '''
					#Azure Container Registry config
					REPO_NAME="nodetest"
					ACR_LOGINSERVER="testrepo1995.azurecr.io"
					ACR_ID="testrepo1995"
					IMAGE_NAME="$ACR_LOGINSERVER/$REPO_NAME:jenkins${BUILD_NUMBER}"

					#Docker build and push to Azure Container Registry
					docker build . -t $IMAGE_NAME 
					docker login $ACR_LOGINSERVER -u $ACR_ID -p ${acrpwd}
					docker push $IMAGE_NAME
					'''


            }

                //sh "docker build . -t rummansadhit/nodetest:${Docker_tag}"
            }
        }

        stage('deploy on k8s'){
		steps{
           	sh '''

            REPO_NAME="nodetest"
			ACR_LOGINSERVER="testrepo1995.azurecr.io"

            KUBE_CONTEXT="testcluster"

            NAME="test"

            HELM_CHART="./k8s/nodetest"


            helm --kube-context $KUBE_CONTEXT upgrade --install --force $NAME $HELM_CHART --set image.repository=$ACR_LOGINSERVER/$REPO_NAME --set image.tag=jenkins${BUILD_NUMBER}

            '''
		}

            }
        }



    }
    



def getDockerTag(){

        def tag = sh script: 'git rev-parse HEAD', returnStdout: true
        return tag
}
