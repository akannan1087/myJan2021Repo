pipeline {
    agent any

    tools {
     maven 'Maven3'
    }
  
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '50f6fddf-d641-43aa-ac88-881339c4ed19', url: 'https://github.com/akannan1087/myJan2021Repo']]])
            }
        }
        
       stage ('Build') {
         steps {
              sh 'mvn clean install -f MyWebApp/pom.xml'
            }
        }
        
        stage ('Code Quality') {
        steps {
            withSonarQubeEnv('My_SonarQube') {
            sh 'mvn -f MyWebApp/pom.xml sonar:sonar'
            }
      }
    }
    
            stage ('Nexus upload') {
                steps {
                           nexusArtifactUploader artifacts: [[artifactId: 'MyWebApp', classifier: '', file: 'MyWebApp/target/MyWebApp.war', type: 'war']], credentialsId: '6ff32036-ec16-4226-9c57-b84ad15d96a5', groupId: 'com.dept.app', nexusUrl: 'ec2-18-221-32-13.us-east-2.compute.amazonaws.com:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
 
                }
        
            }
        
    }
}
