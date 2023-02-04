node
{
def mavenHome = tool name: "maven3.8.7"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "The job name is: ${env.JOB_NAME}"
echo "The node name is: ${env.NODE_NAME}"
echo "The Workspace path is: ${env.WORKSPACE}"
echo "The node lable is: ${env.NODE_LABELS}"
echo "The Build number is: ${env.BUILD_NUMBER}"
  
stage('checkoutcode'){
git credentialsId: 'b15aaeac-ec99-496d-be5f-2ccf3eac7ea5', url: 'https://github.com/ashok9908/maven-web-application.git'
}

stage('mavenbuild'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('SonarqubeScanner'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('atrifactsstorage'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('APPdeployment'){
sshagent(['6a39e6c8-4576-4fd3-8c77-d00d4ad42a86']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.42.156:/opt/apache-tomcat-9.0.71/webapps"
}
}

  
}
