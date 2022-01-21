pipeline {
  agent none
  parameters {
     choice(name: 'BRANCH', choices: ['Main', 'Development'], description: 'Choose branch')
  }
   triggers {
    eventTrigger simpleMatch('hello-api-deploy-event')
  }
  environment {
    FAVORITE_COLOR = 'RED'
  }
  stages {
    stage('Test') {
      //when {
      //  beforeAgent true
      //  not { branch 'main' }
      //}
       echo "Branch Choice: ${params.BRANCH}"
       parallel {
         stage ('Run Test load 1') {
           agent { label 'nodejs-app' }
            steps {
              container('nodejs') {
              echo 'Hello World!'   
              sh 'node --version'
             }
            }
          }
         stage ('Run Test load 2') {
           agent { label 'python-app' }
             steps {
              container('python3') {
              echo 'Hello Python World!'   
              sh 'python3 --version'
             }
            }
          }
       }
      }
   stage('Main Branch Stages') {
      when {
        beforeAgent true
        branch 'main'
      }
      stages {
        stage('Build and Push Image') {
          steps {
            echo "FAVORITE_COLOR is $FAVORITE_COLOR"  
            echo "TODO - build and push image"
          }
        }
        stage('Deploy') {
          agent any
          environment {
            FAVORITE_COLOR = 'BLUE'
            SERVICE_CREDS = credentials('example-service-username-password')
          }
          steps {
            sh 'echo TODO - deploy to $FAVORITE_COLOR with SERVICE_CREDS: username=$SERVICE_CREDS_USR password=$SERVICE_CREDS_PSW'
          }
        }

      }
    }
  }
}
