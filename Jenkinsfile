pipeline {
  agent any
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
    stage('Deploy Apache to VM') {
      steps {
        ansiblePlaybook(
          playbook: '${WORKSPACE}/RandomYeet/playbook.yml',
          inventory: '${WORKSPACE}/RandomYeet/hosts',
          extras: '-vvv'
        )
      }
    }
  }
}
