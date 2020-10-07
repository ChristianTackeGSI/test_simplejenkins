#!groovy

def show_build_change_info(build) {
    def changeLogSets = currentBuild.changeSets
    if (changeLogSets.size() == 0) {
        println "  Empty changeSets?!"
    }
    for (int i = 0; i < changeLogSets.size(); i++) {
        println "  Changeset ${i}:"
        def entries = changeLogSets[i].items
        for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
            println "    ${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
            def files = new ArrayList(entry.affectedFiles)
            for (int k = 0; k < files.size(); k++) {
                def file = files[k]
                println "      ${file.editType.name} ${file.path}"
            }
        }
    }
}


// currentBuild.rawBuild.getParent().setQuietPeriod(300)

String cron_string = ""
Integer quietperiod = 0;
if (BRANCH_NAME == "master") {
  cron_string = "H H 6 * *";
}
if (env.CHANGE_ID == null) {
  quietperiod = 35;
}

pipeline {
  options {
    buildDiscarder(logRotator(numToKeepStr: '12'))
    quietPeriod(quietperiod)
  }
  triggers {
    cron("H H * * *")
  }
  agent none
  stages {
    stage('First stage') {
      steps {
        echo "Start"
        echo "BRANCH_NAME: ${BRANCH_NAME}"
        echo "env.BRANCH_NAME: ${env.BRANCH_NAME}"
        echo "env.CHANGE_ID: ${env.CHANGE_ID}"
        echo "getBuildCauses: ${currentBuild.getBuildCauses()}"
        echo "cron: ${cron_string}"
        echo "quietperiod: ${quietperiod}"
        script {
            println env.CHANGE_ID
            println currentBuild.displayName

            show_build_change_info(currentBuild)
            def parent = currentBuild.getPreviousBuild()
            while (parent != null) {
                println "- ${parent.displayName}, ${parent.result}:"
                show_build_change_info(parent)
                parent = parent.getPreviousBuild()
            }

            node('master') {
                checkout scm
                show_build_change_info(currentBuild)
            }
        }
      }
    }
    stage('timer stage') {
        when {
            triggeredBy 'TimerTrigger'
        }
        steps {
            echo 'By timer'
        }
    }
  }
}
