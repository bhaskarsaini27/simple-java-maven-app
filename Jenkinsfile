pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)

     SERVICE_NAME="simple-java-maven-app"
     ORGANIZATION_NAME="bhaskarsaini27"
     YOUR_DOCKERHUB_USERNAME="sainibha"
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
            sh '''mvn clean package'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                    // sh '/usr/local/bin/kubectl apply -f ${WORKSPACE}/deploy.yaml'
                    sh 'envsubst < ${WORKSPACE}/deploy.yaml | /usr/local/bin/kubectl apply -f -'
          }
      }
   }
}
