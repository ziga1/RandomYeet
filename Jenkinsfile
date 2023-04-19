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
    
    stage('Generate New Certificate') {
      steps {
        sh '''
        
      if [ -f /var/lib/jenkins/.ssh/id_rsa ]; then
        echo "SSH key already exists, skipping the SSH key generation stage"
        return
      fi
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
        sshpass -p ${Password} ssh-copy-id -i ~/.ssh/id_rsa.pub ${Username}@${Hostname}
        '''
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
  }
}
