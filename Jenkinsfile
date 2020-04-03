#!groovy

// currentBuild.rawBuild.getParent().setQuietPeriod(300)

pipeline {
  options {
    quietPeriod(300)
    buildDiscarder(logRotator(numToKeepStr: '12'))
  }
  triggers {
    PeriodicFolderTrigger('1h')
  }
  agent none
  stages {
    stage('First stage') {
      steps {
        echo "Start"
      }
    }
  }
}
