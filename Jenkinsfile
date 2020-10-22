pipeline {
     agent any
     stages {
         
         stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
         stage('Upload to AWS S3') {
            when {
               branch 'master'
            }
              steps {
                  withAWS(region:'us-west-2',credentials:'jenkins') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'udacitysijo2020')
                  }
              }
         }
                 
         stage('Build Docker Image') {
             when {
                branch 'master'
            }
              steps { 
                  sh 'docker build --tag=projectscapstone .'
              }
         }
         stage('upload image to Dockerhub') {
              steps { 
                  script {
                  withDockerRegistry([ credentialsId: "dockerhub", url: "" ]){
                  sh "./upload_docker.sh"
                  }
                  }
              }
         }
        
     }
}
