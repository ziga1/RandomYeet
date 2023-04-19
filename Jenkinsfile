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
        rm /var/lib/jenkins/ssh-certs/id_rsa
        rm /var/lib/jenkins/ssh-certs/id_rsa.pub
        '''
      }
    }
    stage('Generate New Certificate') {
      steps {
        sh '''
          # Generate a new SSH RSA key pair
          ssh-keygen -t rsa -b 4096 -N "" -f id_rsa
        
          # Save the private and public keys to files
          echo "${ssh_private_key}" > /var/lib/jenkins/ssh-certs/id_rsa
          echo "${ssh_public_key}" > /var/lib/jenkins/ssh-certs/id_rsa.pub
        '''
      }
    }
    stage('Adding Public Certificate to Remote Host ') {
      steps {
        sh 'ssh-copy-id -i /var/lib/jenkins/ssh-certs/id_rsa.pub ${Username}@${Hostname}'
      }
    }
    stage('Deploy Apache to VM') {
      steps {
        sh 'cat /var/lib/jenkins/ssh-certs/id_rsa.pem.pub'
      }
    }
  }
}
