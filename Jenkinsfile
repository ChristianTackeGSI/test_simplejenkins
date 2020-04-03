#!groovy

// currentBuild.rawBuild.getParent().setQuietPeriod(300)

String cron_string = ""
if (BRANCH_NAME == "master") {
  cron_string = "H H 6 * *"
}

pipeline {
  options {
    quietPeriod(0)
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
        echo "BRANCH_NAME: ${BRANCH_NAME}"
        echo "CHANGE_ID: ${CHANGE_ID}"
        echo "env.BRANCH_NAME: ${env.BRANCH_NAME}"
        echo "env.CHANGE_ID: ${env.CHANGE_ID}"
        echo "cron: ${cron_string}"
      }
    }
  }
}
