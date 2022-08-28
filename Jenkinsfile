pipeline {
     agent none     
    stages {
        stage('Restore') {
          agent {
          docker { image 'amrragab/project-repo' }
      }
            steps {
                stash includes: 'node_modules/', name: 'node_modules'
            }
        }
          stage('Lint') {
      agent {
        docker { image 'amrragab/project-repo' }
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
