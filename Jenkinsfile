#!groovy

// currentBuild.rawBuild.getParent().setQuietPeriod(300)

String cron_string = ""
if (BRANCH_NAME == "master") {
  cron_string = "H H 6 * *"
}

pipeline {
  options {
    buildDiscarder(logRotator(numToKeepStr: '12'))
    quietPeriod(null)
  }
  triggers {
    cron("H H/3 * * *")
  }
  agent none
  stages {
    stage('First stage') {
      steps {
        echo "Start"
        echo "BRANCH_NAME: ${BRANCH_NAME}"
        echo "env.BRANCH_NAME: ${env.BRANCH_NAME}"
        echo "env.CHANGE_ID: ${env.CHANGE_ID}"
        echo "cron: ${cron_string}"
      }
    }
  }
}
