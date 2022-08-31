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
      
        
      stage("Build image") {
            steps {
              
                    sh 'docker build -t amrragab/siemens-project:${env.BUILD_ID} . '
                    sh 'docker docker push amrragab/siemens-project:${env.BUILD_ID} '
                }
            }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }             
    }
}
