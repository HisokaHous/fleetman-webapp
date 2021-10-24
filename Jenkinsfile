pipeline {
   agent any

   environment { 
     registry = "hisoka1/mine" 
     registryCredential = 'YOUR_DOCKERHUB_USERNAME'  
     SERVICE_NAME = "fleetman-webapp"
     BUILD_ID = "1"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh 'echo No build required for Webapp.'
         }
      }

      stage('Build and Push Image') {
         steps {
                script { 
                    dockerImage = docker.build registry + ":${REPOSITORY_TAG}" 
                }
            } 
      }

      stage('Deploy to Cluster') {
          steps {
            sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
