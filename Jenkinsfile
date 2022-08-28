pipeline {
     agent none     
    stages {
        stage('Restore') {
          agent {
          docker { image 'alexsuch/angular-cli:latest' }
      }
            steps {
                sh ' npm install --package-lock' 
            }
        }
          stage('Lint') {
      agent {
        docker { image 'alexsuch/angular-cli:latest' }
      }
      steps {
        sh 'ng lint'
      }
    }
        stage('Build') {
            steps {
                sh 'ng build'
            }
        }
        stage('Test') {
            steps {
                sh 'ng test'
            }
        }        
        stage('Deploy') {
            steps {
                sh 'rm ../../apps/*'
                sh 'cp ./dist/apps/* ../../apps/'
            }
        }             
    }
}
