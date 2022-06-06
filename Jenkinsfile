node
{
    
def mavenHome = tool name: "maven3.8.4"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))])

stage('checkoutcode')
{
git branch: 'development', credentialsId: 'ca070bd4-0a94-42e0-bb10-a32126f849ae', url: 'https://github.com/Althaf333/maven-web-application.git'
}

stage('build')
{
sh "whoami"
sh "${mavenHome}/bin/mvn clean package"
}

stage('Executesonarqubereport')
{
sh "${mavenHome}/bin/mvn deploy"
}
    
stage('UploadArtifactsintonexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatserver')
{
sshagent(['1bee304d-fcfb-48f2-95f8-391b9166c67f']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war jenkins@3.90.191.19:/opt/apache-tomcat-9.0.63/webapps/"
}
}

stage('sendEmailNotification')
{
emailext body: '''Testing the configurations...

althaf,
devops.

''', subject: 'new pipe line test', to: 'althafhussain412@gmail.com'
}

}
