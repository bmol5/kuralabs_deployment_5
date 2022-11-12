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
            sudo usermod -a -G docker jenkins
            cd ./dockerf
            docker build -t flask:latest .
            '''
        }
    }
    stage ('push') {
      agent{label 'dockerage'}
        steps{
          sh '''#!/bin/bash
          docker tag flask:v1 bmol5/flask:latest
          docker push bmol5/flask:latest
          '''
        }
    }
   }
}
