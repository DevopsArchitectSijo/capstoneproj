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
                  sh 'docker build --tag=projectcapstone .'
              }
         }
         stage('upload image to Dockerhub') {
              steps { 
                  script {
                  withDockerRegistry([ credentialsId: "dockerhub", url: "" ]){
                  sh "docker tag projectcapstone sijodevops/capstoneproj"
                  sh 'docker push sijodevops/capstoneproj'
                  }
                  }
              }
         }
          
          stage('Deploy to AWS Kubernetes Cluster') {
             when {
                branch 'master'
            }
              steps {
                  withAWS(region:'us-west-2',credentials:'jenkins') {
                  sh "aws eks --region us-west-2 update-kubeconfig --name capstoneproject123"
                  sh "kubectl set image deployments/projectcapstone projectcapstone=sijodevops/capstoneproj/projectcapstone:latest"
                  sh "kubectl apply -f Myinfradeployment.yml"
                  sh "kubectl get nodes"
                  sh "kubectl get deployment"
                  sh "kubectl get pod -o wide"
                  sh "kubectl apply -f indexservices.yml"
                  sh "kubectl get services"
                    
                  }
              }
         }
        
     }
}
