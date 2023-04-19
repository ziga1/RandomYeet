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
    stage('Delete Old Certificate') {
      steps {
        sh '''
      if [ -f /var/lib/jenkins/workspace/${Github}/id_rsa ]; then
        rm /var/lib/jenkins/workspace/${Github}/id_rsa
      fi
      
      if [ -f /var/lib/jenkins/workspace/${Github}/id_rsa.pub ]; then
        rm /var/lib/jenkins/workspace/${Github}/id_rsa.pub
      fi
    '''
      }
    }
    stage('Generate New Certificate') {
      steps {
        sh '''
          # Generate a new SSH RSA key pair
          ssh-keygen -t rsa -b 4096 -N "" -f id_rsa
        '''
      }
    }
    stage('Adding Public Certificate to Remote Host ') {
      steps {
        sh "sshpass -p ${Password} ssh-copy-id -i /var/lib/jenkins/workspace/${Github}/id_rsa.pub ${Username}@${Hostname} -vvv"
      }
    }
    stage('Deploy Apache to VM') {
      steps {
        sh 'yay'
      }
    }
  }
}
