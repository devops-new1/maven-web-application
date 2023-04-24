node
{

def mavenHome = tool name: "maven-3.9.0"

try {
slackNotifications("STARTED")
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
} }

} 

catch (e) {
    currentBuild.result = "FAILED"
    throw e
}

finally {
    slackNotifications(currentBuild.result)
}
} // node closing

def slackNotifications(String buildStatus = 'STARTED') {
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
