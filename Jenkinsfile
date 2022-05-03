def sendSlackNotifications(String buildStatus = 'STARTED') {
 
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}

node{

def mavenHome = tool name: 'maven 3.8.5'

try{
 //sendSlackNotifications('STARTED')
//Get the code from githb repo
stage('CheckoutCode'){
git branch: 'development', url: 'https://github.com/aparnamandepudi/maven-web-application.git'
}
//Do the build by using maven build tool
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
/*
//Execute the SonarQube report
stage('Execute sonarqube report'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//upload artifacts into artifactory repo
stage('upload artifacts into nexus'){
sh "${mavenHome}/bin/mvn deploy"
}

//deploy application into tomcat server
stage('deploy application into tomcat server'){
sshagent(['97eef3d6-9ab1-40bd-93a0-0965228aa113']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.194.69:/opt/apache-tomcat-9.0.62/webapps/"
}

}
*/
}
  catch(e){
        currentBuild.result = "FAILED"
  }
  finally{
    //sendSlackNotifications(currentBuild.result)
}
}
