node{

def mavenHome = tool name:"maven3.8.5"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "Job name is: ${env.JOB_NAME}"
echo "Node name is: ${env.NODE_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"

try{
  SendSlackNotifications('STARTED')
  
stage('CheckoutCode')
{
git branch: 'development', credentialsId: '27cbb011-ffd2-4fb6-8984-7c3826a4ffe7', url: 'https://github.com/Shiva-DevOps-Batch/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
/*
stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatserver')
{
sshagent(['5944bcfd-1b7f-4468-a6ee-6476a33a0257']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.214.227:/opt/apache-tomcat-9.0.63/webapps"
}
}
*/
}catch(e){
currentBuild.result = "FAILED"
  throw e
}
  finally{
  SendSlackNotifications(currentBuild.result)
  }
}//node closing

//Below code is used to send the build notifications to Slack

def SendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}


