pipeline {
     agent none     
    stages {
        stage('Restore') {
          agent {
          docker { image 'node:12.7-alpine' }
      }
            steps {
                sh 'npm cache clean --force '
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
