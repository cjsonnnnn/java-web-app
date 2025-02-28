pipeline {
  agent { label 'agent-dind' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-jpiay')
  }
  // parameters { 
  //   string(name: 'APP_NAME', defaultValue: '', description: 'What is the Heroku app name?') 
  // }
  stages {
    // stage('Build') {
    //   steps {
    //     sh 'docker build -t jpiay/jwa:latest .'
    //   }
    // }
    stage('Login') {
      steps {
        sh 'cat /etc/passwd'
        sh 'whoami'
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push jpiay/jwa:latest'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
//      cleanWs()  // Deletes all files in the workspace
    }
  }
}