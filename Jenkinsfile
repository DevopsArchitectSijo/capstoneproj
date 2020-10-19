pipeline {
     agent any
     stages {
         stage('Build') {
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
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
         stage('upload image to Dockerhub') {
              steps { 
                  script {
                  withDockerRegistry([ credentialsId: "dockerhub", url: "" ]){
                  sh "./upload_docker.sh"
                  }
                  }
              }
         }
        /* stage('Security Scan') {
             when {
                branch 'staging'
            }
              steps { 
                  aquaMicroscanner imageName: 'alpine:latest', notCompliesCmd: 'exit 1', onDisallowed: 'fail', outputFormat: 'html'
              }
         }   */      
        /* stage('Upload to AWS') {
             when {
                branch 'master'
            }
              steps {
                  withAWS(region:'us-west-2',credentials:'aws-static') {
                  sh 'echo "Uploading content with AWS creds"'
                      s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'module2lesson5s3')
                  }
              }
         }*/
         stage('Deploy to AWS Kubernetes Cluster') {
             when {
                branch 'master'
            }
              steps {
                  withAWS(region:'us-east-2',credentials:'aws-static') {
                  sh "aws eks --region us-east-2 update-kubeconfig --name capstonecluster"
                  sh "kubectl set image deployments/projectcapstoneindex projectcapstoneindex=vikrantarora14/projectcapstoneindex:latest"
                  sh "kubectl apply -f deployment.yml"
                  sh "kubectl get nodes"
                  sh "kubectl get deployment"
                  sh "kubectl get pod -o wide"
                  sh "kubectl apply -f service.yml"
                  sh "kubectl get services"
                    
                  }
              }
         }
     }
}