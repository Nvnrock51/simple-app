pipeline{
    agent any
    tools {
        maven "Maven 3.6.3"
    }
    environment {
        NEXUS_VERSION = "nexus3.40.1-01"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "3.110.48.86:8081"
        NEXUS_REPOSITORY = "simple-app"
        NEXUS_CREDENTIAL_ID = "nexus3"
    }
    stages{
        stage("GIT checkout"){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Nvnrock51/simple-app.git']]])
                
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war /var/lib/jenkins/workspace/simple-app/target/simpleapp.war"
            }
         }
         stage("Publish to Nexus Repository Manager") {
            steps {
         nexusArtifactUploader artifacts: 
         [
             [
                 artifactId: 'simple-app',
                 classifier: '',
                 file: '/var/lib/jenkins/workspace/simple-app/target/simple-app.war',
                 type: 'war'
                ]
            ],
                 credentialsId: 'nexus3',
                 groupId: 'in.javahome',
                 nexusUrl: '3.110.48.86:8081',
                 nexusVersion: 'nexus3',
                 protocol: 'http', 
                 repository: 'simpleapp-release', 
                 version: '3.0.0'   
          }
    }
  }
}
