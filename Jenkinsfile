node{
     echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
    properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
  ])

def mavenHome=tool name: "maven3.6.3"
stage('Checkout'){

git branch: 'development', credentialsId: '316d57b2-9d49-4e90-a345-187e6fcc3cc3', url: 'https://github.com/sai432/maven-web-application.git'
}
stage('Build'){

sh "${mavenHome}/bin/mvn clean package"
}
stage('SonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}
stage('UploadArtifactoryintoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}
stage('UploadAppintoTomcat'){
sshagent(['4db57cd8-a769-46e8-b54b-ebf9480aa77b']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.207.223.238:/opt/apache-tomcat-9.0.37/webapps/"

}
stage('SendEmailNotification'){
emailext body: '''Build is over..
 
Regards,
kondalarao,
8328151383.''', subject: 'Build is over', to: 'kondalarao7222@gmail.com'
}
}
}
