node{
    
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "The job name is: ${env.JOB_NAME}"
echo "The Node name is: ${env.NODE_NAME}" 
echo "The Build Number is: ${env.BUILD_NUMBER}"  
  
def mavenHome = tool name: "maven3.8.5"

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
    
}
