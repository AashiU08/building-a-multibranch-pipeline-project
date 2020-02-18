pipeline {
  agent {
    docker {
      image 'node:8-alpine'
      args '-p 3000:3000 -p 5000:5000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh'
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
      }
    }

    stage('Deploy for production') {
      parallel {
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

        stage('email') {
          steps {
            emailext(subject: '[Jenkins] building-a-multibranch-pipeline-project', body: '\'\'\'${SCRIPT, template="groovy-html.template"}\'\'\'', attachLog: true, from: 'snigdhaupadhyay08@gmail.com', presendScript: 'this could be used to notify people that a new build is happening build.previousBuild.result.toString().equals(\\\'TRUE\\\')', postsendScript: 'only send an email if the word {{ERROR}} is found in build logs build.logFile.text.readLines().any { it =~ /.*ERROR.*/ }', mimeType: 'type=text/html', to: 'aashi.upadhyay@assetvantage.com')
          }
        }

      }
    }

  }
  environment {
    CI = 'true'
  }
}