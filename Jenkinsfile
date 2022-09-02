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
      
//    stage('Test') {
       
//       steps {
//          sh 'ng test --progress=false --watch false'
//        }

 //       post {
//        always {
//          junit "test-results.xml"
//          }
//      }
//    }
//
           stage('e2e') {
            steps {
                sh 'ng e2e'
            }
        }  
        stage('Build') {
            steps {
                sh 'ng build --prod --progress=false' 
            }
         
    }
        
      stage('Publish'){
        steps {
          sh ' npm publish --registry https://amrjfrogserver.jfrog.io/artifactory/api/npm/npm-local/ '
        }     
  }

      stage("Push") {
       environment {
        imageName = 'siemens-project'
        dockerName = 'amrragab'
      }
            steps {
                       script {
               withDockerRegistry(credentialsId: 'docker', url: 'https://index.docker.io/v1/') {
               def astonvillaimage = docker.build dockerName + "/" + imageName + ":" + "${env.BUILD_NUMBER}"           
               astonvillaimage.push('latest')
               astonvillaimage.push( "release-" + "${env.BUILD_NUMBER}" )
                
                     }
                    }
                  }
                }
   stage('Approval') {
            
            agent none
            steps {
                script {
                    def deploymentDelay = input id: 'Deploy', message: 'Deploy to production?', submitter: 'amrragab,admin', parameters: [choice(choices: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24'], description: 'Hours to delay deployment?', name: 'deploymentDelay')]
                    sleep time: deploymentDelay.toInteger(), unit: 'HOURS'
                }
            }
        }

    stage('Deploy App') {
      steps {
        script {
kubeconfig(caCertificate: '', credentialsId: 'd36be92a-b5dd-4ed9-a6bc-0ec4247d89d1', serverUrl: 'https://10.0.1.220:5443') {
      sh 'kubectl apply -f deploy.yml '
   }        }
      }
    }             
    }
          post {
        failure {
            echo 'Seems like deployment failed, an email will be sent to Jenkins Admin'
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
            
        }
          }
}
