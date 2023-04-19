pipeline {
  agent any
  
  environment {
    remotehost = "my value"
  }
  
  stages {
    stage('Checkout from Git') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: '*/master']],
          userRemoteConfigs: [[url: 'https://github.com/Koncinaa/RandomYeet.git']]
        ])
      }
    }
    stage('Test') {
      steps {
        sh 'echo ${Hostname}'
      }
    }
  }
}
