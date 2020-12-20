node('master'){
    
    echo "GitHub BranchName ${env.BRANCH_NAME}"
    echo "Jenkins Job Number ${env.BUILD_NUMBER}"
    echo "Jenkins Node Name ${env.NAME_NAME}"
    
    echo "Jenkins Name ${env.JENKINS_NAME}"
    echo "Jenkins URL ${env.JENKINS_URL}"
    echo "Job Name ${env.JOB_NAME}"
    
    def mavenHome = tool name: "Maven3.6.3"
    
    properties([
        buildDiscarder(logRotator(numToKeepStr: '3')),
        pipelineTriggers([
            pollSCM('* * * * *')
    
    ])
        
        
    ])
    
stage('Checkout the Code'){
    
    git credentialsId: '82939d94-9c11-4090-8e0b-6431ec22d09c', url: 'https://github.com/sai432/maven-web-application.git'
    
  }
 stage('build the artifactory'){
     
     //sh "mvn clean package"
     sh "${mavenHome}/bin/mvn clean package"
 }
 stage('SonarQube Report'){
     sh "${mavenHome}/bin/mvn sonar:sonar"
 }
 stage('Nexes Report'){
     sh "${mavenHome}/bin/mvn clean deploy"
 }
 sshagent(['b400fd97-190c-4a44-9aca-3f1ee2520962']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.9.49:/opt/apache-tomcat-8.5.61/webapps/"
}

}
stage('Send a Email Notifaction'){
    emailext body: '''Build is Over

Regard,
konnda,
9908711796''', subject: 'Build is Over', to: 'kondalu432@gmail.com,kondalarao0164@gmail.com'
}
    
