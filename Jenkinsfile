pipeline {
     agent any     
    stages {
          stage('Dependencies') {
       steps {
        sh 'sudo npm install -g @angular/cli@7'
        sh 'sudo npm install'
       }
      }
            
          stage('Lint') {

      steps {
        sh 'ng lint'
      }
          }
        stage('Test') {
            steps {
                sh 'ng test--browsers ChromeHeadless  --watch=false'
            }
        }  
            
           stage('e2e') {
            steps {
                sh 'ng e2e'
            }
        }  
        stage('Build') {
            steps {
                sh 'ng build'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'ls'
            }
        }             
    }
}
