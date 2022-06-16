node{

def mavenHome = tool name:"maven3.8.5"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "Job name is: ${env.JOB_NAME}"
echo "Node name is: ${env.NODE_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"

try{
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
