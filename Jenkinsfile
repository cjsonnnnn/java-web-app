def remote=[:]
remote.name = 'vm-lab1'
remote.host = '192.168.18.21'
remote.allowAnyHosts = true

pipeline {
  // agent any
  // agent { label 'agent-maven' }
  // agent { label 'agent-jenkins' }
  agent { label 'agent-dind' }
  // agent { label 'built-in' }
  tools {
      jfrog 'jfrog-cli'
  }
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
    // stage('Preparation') {
    //   steps {
    //     sh 'apt update'
    //     sh 'apt install tree'
    //   }
    // }
    stage('Testing') {
        steps {
            // Show the installed version of JFrog CLI.
            jf '-v'

            // Show the configured JFrog Platform instances.
            jf 'c show'

            // Ping Artifactory.
            jf 'rt ping'

            // Create a file and upload it to a repository named 'example-repo-local' in Artifactory
            sh 'touch test-file'
            // jf 'rt u test-file example-repo-local/'

            // Publish the build-info to Artifactory.
            // jf 'rt bp'

            // Download the test-file
            // jf 'rt dl example-repo-local/test-file'
        }
    }
    // stage('Build ') {
    //   steps {
        // build
        // sh 'docker build -t jpiay/jwa:latest .'
        // sh 'ls -al'
        // sh 'mvn clean package'
        // sh 'ls -al'
        // sh 'ls -al target'
    //   }
    // }
    // stage('Publish to Artifactory') {
    //   steps {
        // upload to Artifactory
    //     jf 'rt bp'
    //     jf 'rt u target/*.jar example-repo-local/'
    //   }
    // }
    stage('Build Docker Image'){
      steps {
        sh 'ls -al'
        jf 'rt dl target/demo-0.0.1-SNAPSHOT.jar --flat=true'
        sh 'ls -al'
        sh 'docker build -t jpiay/jwa:latest .'
        sh 'ls -al'
      }
    }
    // stage('Login') {
    //   steps {
    //     sh 'cat /etc/passwd | sort'
    //     sh 'cat /etc/group | sort'
    //     sh 'whoami'
    //     sh 'pwd'
    //     sh 'ls -al /var/jenkins_home'
    //     sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
    //   }
    // }
    // stage('Push') {
    //   steps {
    //     sh 'docker push jpiay/jwa:latest'
    //   }
    // }
    // stage('Pull and Deploy') {
    //   steps{
    //     script {
    //       remote.user=env.PI_CREDS_USR
    //       remote.password=env.PI_CREDS_PSW
    //     }
    //     sshCommand(remote: remote, command: "sudo docker pull jpiay/jwa:latest")
    //     sshCommand(remote: remote, command: "sudo docker run -d --name java-web-app -p 8090:8080 --restart unless-stopped jpiay/jwa:latest")
    //   }
    // }  
  }
  post {
    always {
      // sh 'docker logout'
      sleep(5)
      cleanWs()  // Deletes all files in the workspace
    }
  }
}