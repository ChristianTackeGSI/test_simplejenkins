#!groovy

// currentBuild.rawBuild.getParent().setQuietPeriod(300)

pipeline {
  options {
    quietPeriod(1800)
    buildDiscarder(logRotator(numToKeepStr: '12'))
  }
  triggers {
    cron("H/10 * * * *")
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
