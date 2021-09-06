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
                sshCommand remote: remote, command: 'cp -a /var/www/html/. /home/stage/backup/'
                sshCommand remote: remote, command: 'cd /home/stage/backup'
                sshCommand remote: remote, command: 'ls -lrt'
            }
        }
        stage('Deploy') {
            steps {
                sshCommand remote: remote, command: 'git clone https://github.com/kshitijwalukar/Sample-Website.git'
                sshCommand remote: remote, command: 'cd /var/www/html/'
                sshCommand remote: remote, command: 'ls -lrt'
            }
        }
      stage('Cucumber') {
          steps {
            cucumber '**/*.json'
          }
        }
    }
}
