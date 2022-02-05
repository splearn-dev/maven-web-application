node{
    
    def mavenHome = tool name : "maven 3.8.2"
    stage('CheckOutCode')
    {
    git branch: 'development', credentialsId: 'e9da15db-1054-4be4-8ca8-246f1ca402fb', url: 'https://github.com/splearn-dev/maven-web-application.git'
    }
    stage('Build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('SonarQubeReport')
    {
      sh "${mavenHome}/bin/mvn clean sonar:sonar"   
    }
    stage('UploadArtifactsintoNexus')
    {
         sh "${mavenHome}/bin/mvn clean deploy"
    }
    stage('DeplyInToTomcatServer')
{
sshagent(['f84ff74c-c358-49bb-aaae-ff1759706d35']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.110.173.98:/opt/apache-tomcat-9.0.56/webapps/"
}
}
stage('SendEmailNotification')
{
    emailext body: '''Build is over !!
Regards,
s.prakash
9491177553''', subject: 'Build over...!!', to: 'gaterank@gmail.com'
}
}
