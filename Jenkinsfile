pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000 -p 5000:5000'
    }

  }
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'npm install'
          }
        }

        stage('email') {
          steps {
            emailext(subject: 'jenkins multibranch pipeline', body: 'the first build stage has been executed ', from: 'snigdhaupadhyay08@gmail.com', replyTo: 'snigdhaupadhyay08@gmail.com', to: 'aashi.upadhyay@assetvanatge.com')
          }
        }

      }
    }

    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            sh './jenkins/scripts/test.sh'
          }
        }

        stage('email') {
          steps {
            emailext(subject: 'Jenkins multibranch pipeline ', body: 'Test stage has been executed ', compressLog: true, from: 'snigdhaupadhyay08@gmail.com', replyTo: 'snigdhaupadhyay08@gmail.com', saveOutput: true, to: 'aashi.upadhyay@assetvantage.com')
          }
        }

      }
    }

  stage('Deliver for development') {
      parallel {
        stage('Deliver for development') {
          when {
            branch 'development'
          }
          steps {
            sh './jenkins/scripts/deliver-for-development.sh'
            input 'Finished using the web site? (Click "Proceed" to continue)'
            sh './jenkins/scripts/kill.sh'
          }

        stage('email') {
           steps {
            emailext(subject: '[Jenkins] building-a-multibranch-pipeline-project', body: 'Deliver for development is executed', attachLog: true, from: 'snigdhaupadhyay08@gmail.com', to: 'aashi.upadhyay@assetvantage.com', replyTo: 'snigdhaupadhyay08@gmail.com', saveOutput: true)
           }
          }     
        }
      } 
    } 
    
    stage('Deploy for production') {
          when {
            branch 'production'
          }
          steps {
            sh './jenkins/scripts/deploy-for-production.sh'
            input 'Finished using the web site? (Click "Proceed" to continue)'
            sh './jenkins/scripts/kill.sh'
          } 
        }
  }

  environment {
    CI = 'true'
  }
}