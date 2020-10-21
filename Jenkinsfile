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
       
        
     }
}
