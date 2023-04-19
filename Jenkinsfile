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
    stage('Delete Old Certificates') {
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
        sh '''
        ssh-keyscan ${Hostname} >> ~/.ssh/known_hosts
        sshpass -p ${Password} ssh-copy-id -i ~/.ssh/id_rsa.pub ${Username}@${Hostname}'''
      }
    }
    stage('Deploy Apache to Remote VM') {
      steps {
        ansiblePlaybook(
          playbook: '${WORKSPACE}/playbook.yml',
          inventory: '${WORKSPACE}/hosts'
        )
      }
    }
    stage('Jenkins Clean Up') {
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
      
      rm ~/.ssh/ssh-copy-id.*
    '''
      }
    }
  }
}
