node{

def mavenHome = tool name: 'maven 3.8.5'


//Get the code from githb repo
stage('CheckoutCode'){
git branch: 'development', url: 'https://github.com/aparnamandepudi/maven-web-application.git'
}
//Do the build by using maven build tool
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

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
}
