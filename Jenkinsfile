#!groovy

properties([
    pipelineTriggers([[$class: 'PeriodicFolderTrigger', interval: '2h']])
])

pipeline {
  options {
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
