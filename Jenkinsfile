pipeline {
  agent {
    docker {
      image 'node:8-alpine'
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
            emailext(subject: 'jenkins multibranch pipeline', body: 'the first build stage has been executed ', from: 'snigdhaupadhyay08@gmail.com', replyTo: 'snigdhaupadhyay08@gmail.com', to: 'aashi.upadhyay@assetvanatge.com', postsendScript: 'build.result.toString().equals(\'FAILURE\')')
          }
        }

      }
    }

    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh'
        emailext(subject: 'test stage ', body: 'The test stage is being executed', attachLog: true, replyTo: 'snigdhaupadhyay08@gmail.com', to: 'aashi.upadhyay@assetvantage.com', from: 'snigdhaupadhyay08@gmail.com')
      }
    }

    stage('Deliver for development') {
      when {
        branch 'development'
      }
      steps {
        sh './jenkins/scripts/deliver-for-development.sh'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
        emailext(subject: 'Jenkins test step ', body: 'Test step is executed ', attachLog: true, from: 'snigdhaupadhyay08@gmail.com', replyTo: 'snigdhaupadhyay08@gmail.com', to: 'aashi.upadhyay@assetvantage.com')
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