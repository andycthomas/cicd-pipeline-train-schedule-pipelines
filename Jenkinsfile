pipeline {
  agent any
  stages {
    stage ('Build'){
      steps {
        echo 'Running build automation'
        sh './gradlew build --no-daemon'
        archiveArtifcats artifcats; 'dist/trainSchedule.zip'
      }

     stage ('DeployToStaging'){
      when {
        branch 'master'
      }

      steps {
        withCredentials( [usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable:)])
          sshPublisher(
            failOnError: true,
            continueOnError: false,
            publishers: {
              sshPublisherDesc(
                configName: 'staging',
                sshCredentials: [
                  username: "$USERNAME",
                  encryptedPassphrase: "USERPASS"
                ],
                transfers: [
                  sshTransfer(
                    sourceFiles: 'dist/trainSchedule.zip',
                    removePrefix: 'dist/',
                    remoteDirectory: '/tmp',
                    execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule')
                  ]
                )
            }
          )
      }
     }
    }
  }
}

