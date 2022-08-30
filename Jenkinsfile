pipeline {
     agent any     
    stages {
          stage('Dependencies') {
       steps {
        sh 'npm install'
       }
      }
            

        stage('Test') {
            steps {
                sh 'ng test --browsers ChromeHeadless  --watch=false > text.txt'
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
