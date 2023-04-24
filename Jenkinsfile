node
{

def mavenHome = tool name: "maven-3.9.0"

stage ('Checkout Code') {
git branch: 'development', url: 'https://github.com/devops-new1/maven-web-application.git'
}
stage ('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
stage ('Execute SonarQube Report') {
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage ('Upload Artifcats into Nexus') {
sh "${mavenHome}/bin/mvn deploy"
}
stage ('Deploy in Tomcat Server'){
sshagent(['cf00e5bf-20c3-4ebe-a34a-18286d6c626a']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@43.205.233.50:/opt/apache-tomcat-9.0.73/webapps"
}

}
}  
