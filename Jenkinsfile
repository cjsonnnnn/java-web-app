def remote=[:]
remote.name = 'vm-lab1'
remote.host = '192.168.18.21'
remote.allowAnyHosts = true

pipeline {
  agent { label 'agent-dind' }
  // agent { label 'built-in' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-jpiay')
    PI_CREDS=credentials('ssh-vm-lab1')
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
        // sh 'cat /etc/passwd | sort'
        // sh 'cat /etc/group | sort'
        // sh 'whoami'
        // sh 'pwd'
        // sh 'ls -al /var/jenkins_home'
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    // stage('Push') {
    //   steps {
    //     sh 'docker push jpiay/jwa:latest'
    //   }
    // }
    stage('Hello') {
      steps{
        script {
          remote.user=env.PI_CREDS_USR
          remote.password=env.PI_CREDS_USR
        }
        echo 'Hello, Jason!!'
        sshCommand(remote: remote, command: 'ls -la')
      }
    }
  }
  post {
    always {
      sh 'docker logout'
      sleep(5)
//      cleanWs()  // Deletes all files in the workspace
    }
  }
}