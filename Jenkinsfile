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
          sh 'ng test --progress=false --watch false'
        }
       
        post {
          always {
            junit "test-results.xml"
          }
      }
    }

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
        }
      stage('Artifacts'){
        steps {
          sh ' npm publish --registry https://amrjfrogserver.jfrog.io/artifactory/api/npm/npm-local/ '
        }     
  }

      stage("Build image") {
            steps {
              
                    sh 'docker build -t amrragab/siemens-project:latest . '
                    sh 'docker push amrragab/siemens-project:latest '
                }
            }

    stage('Deploy App') {
      steps {
        script {
kubeconfig(caCertificate: '''-----BEGIN CERTIFICATE-----
MIIDDzCCAfegAwIBAgIBADANBgkqhkiG9w0BAQsFADApMRkwFwYDVQQKExBDQ0Ug
VGVjaG5vbG9naWVzMQwwCgYDVQQDEwNDQ0UwHhcNMjIwODE0MjAwMDAwWhcNNDIw
ODE0MjAwMDAwWjApMRkwFwYDVQQKExBDQ0UgVGVjaG5vbG9naWVzMQwwCgYDVQQD
EwNDQ0UwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCUzE19Gdu5RCLn
9cB/bMSNLcO7fy7tjuL6pIj/CQKzRhgi8c9xTbumWUcZdw7ueAMJvyvx7Qxrvaqq
oDZo7hNDYHKaOzXcRghLjIaggBas2SeRcZO9gf3hYaVy53vv/JN4Je7qvHhBxpfs
UwvqF5Q/vbJJuKBgA/A+kFSjWQE2LTOvQbQDPu1S7+W62rNkzDLbJ1xKe13f7a/L
llqp7ZPnqRB53zCL8acWOS/SMYtsD46Zytp82qJYz3JdFOCDDpTQ6vBwccHHhCHP
ryoHrjlLTiBe+ghRSmh7LVT+yjrRDZNWsZZLPTj1bYrrBFo+nFnYYY5v71ad63af
pEbFMnktAgMBAAGjQjBAMA4GA1UdDwEB/wQEAwICpDAPBgNVHRMBAf8EBTADAQH/
MB0GA1UdDgQWBBT/HKsmge8Xy3tHMWNnsEUoiHUW5zANBgkqhkiG9w0BAQsFAAOC
AQEAF4G2O4uD01rbU2sXTT4Jry/zWAQZKkNEyRHNg+MBTdsfOInsMK2otvhHhyi5
cM3EAMNy1+36Wzc3fGaRk1FwXkdZaOc1jigHRPy01LbSyVA59lcKWss0diHD25us
mcFWp5CW6h0FnvsI0tnwkxaH1kblPmWjzzCh9x/Rc5EQWLV/oKIGdCeRolTPFW7k
LJBj2y12SWy5Xd0+MMLNJX4CEeOpzRi7QlBAQa8yRuXGo6wRxivRg60Z5y41TZyb
zL00LtZ8m6ACm+PsmOU+Hs+hwjhEIZunYEMeoYAYwuAO2z6NK9HGtQBcIrKU9GNX
a2qdM+LMO2DrWjjqQnxPy8/vkA==
-----END CERTIFICATE-----''', credentialsId: 'd36be92a-b5dd-4ed9-a6bc-0ec4247d89d1', serverUrl: 'https://10.0.1.220:5443') {
      sh 'kubectl apply -f deploy.yml '
   }        }
      }
    }             
    }
          post {
        failure {
            echo 'I will always say Hello again!'
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
            
        }
}
