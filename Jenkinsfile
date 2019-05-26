pipeline {
  agent any
  stages {
    stage ('Build'){
      steps {
        echo 'Running build automation'
        sh './gradlew build --no-daemon'
        archiveArtifcats artifcats; 'dist/trainSchedule.zip'
      }
    }
  }
}
