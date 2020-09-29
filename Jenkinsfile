def fullBranchUrl(branchName) { return "${scm.getUserRemoteConfigs()[0].getUrl()}/tree/$branchName" }

pipeline {
  agent any
  stages {
    stage('Code Coverage') {
      steps {
        sh 'echo Hello'
      }
      post {
        success {
          script {
            // commit: record code coverage
            if (env.CHANGE_ID == null) {
              currentBuild.result = 'SUCCESS'
              step([$class: 'MasterCoverageAction', scmVars: [GIT_URL: scm.getUserRemoteConfigs()[0].getUrl()]])
            }
            // pull request: compare code coverage
            else if (env.CHANGE_ID != null) {
              currentBuild.result = 'SUCCESS'
              step([$class: 'CompareCoverageAction', publishResultAs: 'statusCheck', scmVars: [GIT_URL: fullBranchUrl(env.CHANGE_TARGET)]])
            }
          }
        }
        cleanup {
          cleanWs()
        }
      }
    }
  }
}
