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
      
      if [ -f /var/lib/jenkins/.ssh/id_rsa.pub ]; then
        rm /var/lib/jenkins/.ssh/id_rsa.pub
      fi
      
      if [ -f /var/lib/jenkins/.ssh/id_rsa ]; then
        rm /var/lib/jenkins/.ssh/id_rsa
      fi
    '''
      }
    }
    stage('Generate New Certificate') {
      steps {
        sh '''
          ssh-keygen -t rsa -b 4096 -N "" -f id_rsa
          mv id_rsa /var/lib/jenkins/.ssh/id_rsa
          mv id_rsa.pub /var/lib/jenkins/.ssh/id_rsa.pub
        '''
      }
    }
    stage('Adding Public Certificate to Remote Host ') {
      steps {
        sh "sshpass -p ${Password} ssh-copy-id ${Username}@${Hostname} -vvv"
      }
    }
    stage('Deploy Apache to VM') {
      steps {
        sh 'yay'
      }
    }
  }
}
