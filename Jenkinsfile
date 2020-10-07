#!groovy

def show_build_change_info(build) {
    def changeLogSets = currentBuild.changeSets
    def resultlist = []
    if (changeLogSets.size() == 0) {
        resultlist.add("  Empty changeSets?!")
    }
    for (int i = 0; i < changeLogSets.size(); i++) {
        resultlist.add("  Changeset ${i}:")
        def entries = changeLogSets[i].items
        for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
            resultlist.add("    Commit: ${entry.commitId}")
            resultlist.add("    by:     ${entry.author}")
            resultlist.add("    on:     ${new Date(entry.timestamp)}")
            resultlist.add("    Text:")
            resultlist.add("    | ${entry.msg}")
            def files = new ArrayList(entry.affectedFiles)
            for (int k = 0; k < files.size(); k++) {
                def file = files[k]
                resultlist.add("      ${file.editType.name} ${file.path}")
            }
        }
    }
    return resultlist.join('\n')
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
    stage('Do a checkout on master') {
      steps {
            node('master') {
                checkout scm
            }
      }
    }
    stage('First stage') {
      steps {
        echo "Start"
        echo "BRANCH_NAME: ${BRANCH_NAME}"
        echo "env.BRANCH_NAME: ${env.BRANCH_NAME}"
        echo "env.CHANGE_ID: ${env.CHANGE_ID}"
        echo "env.CHANGE_TARGET: ${env.CHANGE_TARGET}"
        echo "getBuildCauses: ${currentBuild.getBuildCauses()}"
        echo "cron: ${cron_string}"
        echo "quietperiod: ${quietperiod}"
        script {
            def change_info = ""

            change_info = show_build_change_info(currentBuild)
            println "- ${currentBuild.displayName}\n${change_info}"

            def parent = currentBuild.getPreviousBuild()
            while (parent != null) {
                change_info = show_build_change_info(parent)
                println "- ${parent.displayName}, ${parent.result}:\n${change_info}"
                parent = parent.getPreviousBuild()
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
