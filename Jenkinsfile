#!groovy

// currentBuild.rawBuild.getParent().setQuietPeriod(300)

pipeline {
  options {
    pipelineTriggers([[$class: 'PeriodicFolderTrigger', interval: '2h']])
    buildDiscarder(logRotator(numToKeepStr: '12'))
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
