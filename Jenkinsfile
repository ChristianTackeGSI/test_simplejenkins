#!groovy

// currentBuild.rawBuild.getParent().setQuietPeriod(300)

pipeline {
  options {
    quietPeriod(300)
    buildDiscarder(logRotator(numToKeepStr: '12'))
  }
  triggers {
    cron("H * * * *")
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
