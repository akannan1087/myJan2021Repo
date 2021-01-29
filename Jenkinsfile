pipeline {
    
    agent any
    
     tools {
        maven 'Maven3'
     }
    stages {
        
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '1fcaea7d-050f-44b0-b8eb-2e1af682a9fd', url: 'https://github.com/akannan1087/myJan2021Repo']]])
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
    
    stage ('NExus upload')
    {
        steps {
                 nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: 'ec2-18-191-133-237.us-east-2.compute.amazonaws.com:8081',
                  groupId: 'myGroupId',
                  version: '1.0-SNAPSHOT',
                  repository: 'maven-snapshots',
                  credentialsId: '02b4be4e-6f3e-4f58-9978-31c0a575c2db',
                  artifacts: [
                  [artifactId: 'MyWebApp',
                  classifier: '',
                  file: 'MyWebApp/target/MyWebApp.war',
                  type: 'war']
                  ])
                  }
            }
    
    stage ('DEV Deploy') {
        steps {
            deploy adapters: [tomcat9(credentialsId: 'b7d65b8a-5f84-4eda-97a1-8a7cc9ae8d34', path: '', url: 'http://ec2-3-140-102-164.us-east-2.compute.amazonaws.com:8080')], contextPath: null, war: '**/*.war'
        }
    }
    
     stage ('Slack Notification') {
      steps {
        echo "deployed to DEV Env successfully"
        slackSend(channel:'my-devops-team-channel', message: "Job is successful, here is the info - Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      }
    }
    
     stage ('DEV Approve') {
      steps {
      echo "Taking approval from DEV Manager for QA Deployment"
        timeout(time: 7, unit: 'DAYS') {
        input message: 'Do you want to deploy?', submitter: 'admin'
        }
      }
    }

    }
}
