pipeline {
     agent any
     stages {
         
         stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
          
         stage('Build Docker Image') {
             when {
                branch 'master'
            }
              steps { 
                  sh "./run_docker.sh"
              }
         }
          
         /*stage('upload image to Dockerhub') {
              steps { 
                  script {
                  withDockerRegistry([ credentialsId: "dockerhub", url: "" ]){
                  sh "./upload_docker.sh"
                  }
                  }
              }
         }
                     
          stage('Upload to AWS S3') {
             when {
                branch 'master'
            }
              steps {
                  withAWS(region:'us-west-2',credentials:'aws-static') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'udacitysijo2020')
                  }
              }
         }
         stage('Deploy to AWS Kubernetes Cluster') {
             when {
                branch 'master'
            }
              steps {
                  withAWS(region:'us-east-2',credentials:'aws-static') {
                  sh "aws eks --region us-east-2 update-kubeconfig --name capstoneproj123"
                  sh "kubectl set image deployments/capstoneproj123 projectcapstone=sijodevops/udacityproj/capstoneprojindex:latest"
                  sh "kubectl apply -f deployment.yml"
                  sh "kubectl get nodes"
                  sh "kubectl get deployment"
                  sh "kubectl get pod -o wide"
                  sh "kubectl apply -f indexservices.yml"
                  sh "kubectl get services"
                    
                  }
              }
         }*/
     }
}
