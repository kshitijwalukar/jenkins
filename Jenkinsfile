def remote = [:]
remote.name = "stage"
remote.host = "192.168.245.128"
remote.allowAnyHosts = true
withCredentials([usernamePassword(credentialsId: 'fe95e991-94b8-4103-a575-7c6c6bf277f9', passwordVariable: 'password', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.password = password
}
pipeline {
  agent any
  stages {
    
    stage('Backup') {
            steps {
                sshCommand remote: remote, command: 'rm -rf /home/stage/backup/*'
                sshCommand remote: remote, command: 'cp -a /var/www/html/. /home/stage/backup/'
                sshCommand remote: remote, command: 'cd /home/stage/backup'
                sshCommand remote: remote, command: 'ls -lrt'
            }
        }
        stage('Deploy') {
            steps {
                sshCommand remote: remote, command: 'rm -rf /home/stage/backup/jenkins'
                sshCommand remote: remote, command: 'cd /home/stage/'
                sshCommand remote: remote, command: 'git clone https://github.com/kshitijwalukar/jenkins.git'
                sshCommand remote: remote, command: 'rm -rf /var/www/html/*'
                sshCommand remote: remote, command: 'mv  /home/stage/jenkins/. /var/www/html/jenkins/'
                sshCommand remote: remote, command: 'cd /var/www/html/jenkins'
                sshCommand remote: remote, command: 'ls -lrt'
            }
        }
    stage('Test') {
      parallel {
        stage('Maven') {
          steps {
            echo 'Running from Jenkins file'
            bat(script: 'mvn compile', label: 'maven')
          }
        }

        stage('Cucumber') {
          steps {
            cucumber '**/*.json'
          }
        }

      }
    }

  }
}
