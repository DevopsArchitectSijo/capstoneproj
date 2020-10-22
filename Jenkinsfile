pipeline {
     agent any
     stages {
         
         stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
          
         stage('Lint Dockerfile') {
            steps {
                script {
                    docker.image('hadolint/hadolint:latest-debian').inside() {
                            sh 'hadolint ./Dockerfile | tee -a hadolint_lint.txt'
                            sh 'hadolint ./Dockerfile'
                            sh '''
                                lintErrors=$(stat --printf="%s"  hadolint_lint.txt)
                                
                                if [ "$lintErrors" -gexit "0" ]; then
                                    echo "Errors have been found, please see below"
                                    cat hadolint_lint.txt
                                    exit 1
                                    
                                else
                                    echo "There are no erros found on Dockerfile!!"
                                fi
                            '''
                    }
                }
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
                  sh "kubectl apply -f deployment.yml"
                  sh "kubectl get nodes"
                  sh "kubectl get deployment"
                  sh "kubectl get pod -o wide"
                  sh "kubectl apply -f indexservices.yml"
                  sh "kubectl get services"
                    
                  }
              }
         }
          
         stage("Cleaning up") {
              steps{
                    echo 'Cleaning up...'
                    sh "docker system prune"
              }
        } 
        
     }
}
