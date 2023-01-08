node{
    
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "The job name is: ${env.JOB_NAME}"
echo "The Node name is: ${env.NODE_NAME}" 
echo "The Build Number is: ${env.BUILD_NUMBER}"  
  
def mavenHome = tool name: "maven3.8.5"

try{    
sendslacknotifications("STARTED")    
stage('CheckoutCode'){
git branch: 'development', credentialsId: 'b89b0a18-712f-4925-b13f-dfc9de7d29d6', url: 
'https://github.com/vasanthdevops-program/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
/*
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsintoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppintoTocatServer'){
sshagent(['61b279de-3757-45db-9295-ed16a1e9137b']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.2.168:/opt/apache-tomcat-9.0.70/webapps/"
}
}

*/
}//try closing
catch(e){
currentBuild.result = "FAILURE"
}//catch block closing
finally{

}//finally closing    
sendslacknotifications(currentBuild.result)    
}//node closing

// Slack Send notifications

def sendslacknotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    colorName = 'ORANGE'
    colorCode = '#FFA500'
  } else if (buildStatus == 'SUCCESS') {
    colorName = 'GREEN'
    colorCode = '#00FF00'
  } else {
    colorName = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: "#walmart")
}
