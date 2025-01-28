node{
def mavenHome = tool name: 'maven3.9.9'			//#mavenHome=/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven3.9.9
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])]) 		//For deleting old build and poll scm
stage('CheckOutCOde'){
git branch: 'development', credentialsId: '46833e5c-4d74-4e4b-8148-1a746c7c3393', url: 'https://github.com/mithuntechnologies-bank2024/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSomarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadBuildArtifacts'){
sh "${mavenHome}/bin/mvn deploy"

}

stage('DeployApplicationToTomcatServer'){
sshagent(['d20a6e92-e837-4f24-be6d-376137887be0']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.24.32:/opt/tomcat9/webapps/"
}
}


}
