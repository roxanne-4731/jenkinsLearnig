import hudson.Util;

def notifySlack(String buildStatus = 'STARTED') {
    // Build status of null means success.
    buildStatus = buildStatus ?: 'SUCCESS'

    def color
    if (buildStatus == 'STARTED') {
        color = '#D4DADF'
    } else if (buildStatus == 'SUCCESS') {
        color = '#BDFFC3'
    } else if (buildStatus == 'UNSTABLE') {
        color = '#FFFE89'
    } else {
        color = '#FF9FA1'
    }

    def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL} in ${Util.getTimeSpanString(System.currentTimeMillis() - currentBuild.startTimeInMillis)}"

    slackSend(color: color, message: msg)
}

node {
    def dockerContainerName = "react_app"
  try {
      notifySlack()
        stage('Preparation') {
                checkout scm
                }
       stage('Build') {
       // name of created Nodejs in Global Tool Configuration
          nodejs(nodeJSInstallationName: 'nodejs') {
              sh 'npm install'
              sh 'npm start'
            }
          }
       stage('Deploy') {

          }

  } catch(e) {
    currentBuild.result = "FAILURE";
    throw e;
  } finally {
        notifySlack(currentBuild.result)
  }
}
