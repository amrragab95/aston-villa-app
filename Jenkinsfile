pipeline {
     agent any     
    stages {
          stage('Dependencies') {
       steps {
        sh 'npm install'
       }
      }
           stage('lint') {
       steps {
        sh 'ng lint '
       }
      }           

        stage('Test') {
            steps {
                sh 'ng test --browsers ChromeHeadless '
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
