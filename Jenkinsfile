pipeline {
  agent any 

  stages {
    stage('Daily Compliance Run') {
      steps{
        echo 'Running a compliance scan with inspec....'
          script{
            def remote = [:]
            remote.name = "controlnode"
            remote.host = "10.1.0.128"
            remote.allowAnyHosts = true

            withCredentials([sshUserPrivateKey(credentialsId: 'sshUser', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                remote.user = userName
                remote.identityFile = identity
                stage("Enforce with Ansible") {
                  sshCommand remote: remote, command: 'cd /home/devops/secops/ansible && git pull origin'
                  sshCommand remote: remote, command: 'cd /home/devops/secops/ansible && ansible-playbook compliance.yaml'
              }
                /*stage("Placeholder Stage...") {
                  sshCommand remote: remote, sudo: true, command: 'echo "add your stuff here....."'
                  sshCommand remote: remote, sudo: true, command: 'echo "some more stuff goes here....."'
              }*/              
                stage("Scan with InSpec") {
                  // sshCommand remote: remote, sudo: true, command: 'inspec exec /home/devops/linux-baseline/'
                  sshCommand remote: remote, command: 'inspec exec /home/devops/linux-baseline/'
              }
            }
          }
       }
    }
  }
}

