pipeline {
  agent any
   stages {
    stage ('Build') {
      steps {
        sh '''#!/bin/bash
        python3 -m venv test3
        source test3/bin/activate
        pip install pip --upgrade
        pip install -r requirements.txt
        export FLASK_APP=application
        flask run &
        '''
     }
   }
    stage ('test') {
      steps {
        sh '''#!/bin/bash
        source test3/bin/activate
        py.test --verbose --junit-xml test-reports/results.xml
        ''' 
      }
    
      post{
        always {
          junit 'test-reports/results.xml'
        }
       
      }
    }
   
    stage ('image') {
      agent{label 'dockerage'}
        steps {
          sh '''#!/bin/bash
            sudo usermod -a -G docker $USER
            newgrp docker
            cd ./dockerf
            docker build -t urlshort:latest .
            '''
        }
    }
    stage ('push') {
      agent{label 'dockerage'}
        steps{
          withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
          sh '''#!/bin/bash
          docker tag urlshort:latest bmol5/urlshort:latest
          docker push bmol5/urlshort:latest
          '''
        }
    }
   }
       stage('Init') {
       agent{label 'terrage'}
        steps {
        withCredentials([string(credentialsId: 'AWS_ACCESS_KEY', variable: 'aws_access_key'), 
                        string(credentialsId: 'AWS_SECRET_KEY', variable: 'aws_secret_key')]) {
                            dir('intTerraform') {
                              sh 'terraform init' 
                            }
         }
    }
   }
      stage('Plan') {
      agent{label 'terrage'}  
       steps {
        withCredentials([string(credentialsId: 'AWS_ACCESS_KEY', variable: 'aws_access_key'), 
                        string(credentialsId: 'AWS_SECRET_KEY', variable: 'aws_secret_key')]) {
                            dir('intTerraform') {
                              sh 'terraform plan -out plan.tfplan -var="aws_access_key=$aws_access_key" -var="aws_secret_key=$aws_secret_key"' 
                            }
         }
    }
   }
//       stage('Apply') {
//       agent{label 'terrage'}
//        steps {
//         withCredentials([string(credentialsId: 'AWS_ACCESS_KEY', variable: 'aws_access_key'), 
//                         string(credentialsId: 'AWS_SECRET_KEY', variable: 'aws_secret_key')]) {
//                             dir('intTerraform') {
//                               sh 'terraform apply plan.tfplan' 
//                             }
//          }
//        }
//       }
//       stage('Destroy') {
//       agent{label 'terrage'} 
//       steps {
//         withCredentials([string(credentialsId: 'AWS_ACCESS_KEY', variable: 'aws_access_key'),
//         string(credentialsId: 'AWS_SECRET_KEY', variable: 'aws_secret_key')]) {
//         dir('intTerraform') {
//         sh 'terraform destroy -auto-approve -var="aws_access_key=$aws_access_key" -var="aws_secret_key=$aws_secret_key"'
//       }
//     }
//   }
// }
   }
}
