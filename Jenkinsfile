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
    
            stage ('Code Quality') {
                steps {
                    
                }
        
            }
        
    }
}
