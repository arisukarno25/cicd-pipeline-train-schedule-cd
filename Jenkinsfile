pipeline {
    agent any
    stages {
        stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                   sshPublisher(
                      failOnError: true,
                      continueOnError: false,
                      publishers: [
                         sshPublisherDesc(
                            configName: 'staging',
                            sshCredentials: [
                                 username: '$USERNAME',
                                 encryptedPassphrase: '$USERPASS'
                             ],
                             transfers: [
                                sshTransfer(
                                   sorceFiles: 'dist/trainSchedule.zip',
                                    removePrefix: 'dist/',
                                    remoteDirectory: '/tmp',
                                    execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                )
                             ]
                         )
                      ]
                   )
                }
            }
        }
    }
}
