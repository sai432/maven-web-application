node('master'){
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
  
def mavenHome = tool name: "maven3.6.3"
stage('Checkout Code'){
   git credentialsId: '6b54cc0b-ed10-4348-a8e4-007c405bb7b8', url: 'https://github.com/sai432/maven-web-application.git'
    }
stage('Build '){
	sh "${mavenHome}/bin/mvn clean package"
	}
	stage('SonarQube Report'){
	sh "${mavenHome}/bin/mvn sonar:sonar"
	}
stage('Store Nexus reso'){
	sh "${mavenHome}/bin/mvn deploy"
	}
	stage('deploy to war file tomcat'){

   sshagent(['a59ab86b-6b3a-4033-a108-6051f5de40f1']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.207.113.165:/opt/apache-tomcat-9.0.39/webapps/"

}
}
stage('Sending Email Notification'){
emailext body: '''Build is Over
Regards
kondalarao
9908711796''', subject: 'Build is Over', to: 'kondalu432@gmail.com'

}

}
