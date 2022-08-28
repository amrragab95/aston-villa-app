pipeline {
     agent none     
    stages {
        stage('Restore') {
          agent {
        docker 'node:14.2.0-alpine3.11'
      }
            steps {
                sh 'npm install'
                sh 'npm i -g @angular/cli'
                stash includes: 'node_modules/', name: 'node_modules'
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
