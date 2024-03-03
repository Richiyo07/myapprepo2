pipeline {
    
   agent any
   
   tools {
    maven 'maven3' 
   }
    stages {
        
       
        stage('Checkout') {
       steps {
        checkout scm
        }
    }    
        
        stage ('Build') {
        steps {    
         sh 'mvn clean install -f MyWebApp/pom.xml'
           }
        }
    
        stage('code quality') {
          steps {
             withSonarQubeEnv('SonarQube') {
           sh 'mvn sonar:sonar -f MyWebApp/pom.xml'
             }
          }  
        }
    
        stage('Nexus Upload') {   
            steps { 
                nexusArtifactUploader(
      nexusVersion: 'nexus3',
      protocol: 'http',
      nexusUrl: 'ec2-34-219-13-3.us-west-2.compute.amazonaws.com:8081/',
      groupId: 'myGroupId',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: '165cf6be-04ae-4a02-8226-b02b30097c6e',
      artifacts: [
      [artifactId: 'MyWebApp',
      classifier: '',
      file: 'MyWebApp/target/MyWebApp.war',
      type: 'war']
      ])   
            } 
        } 
    
    stage('DEV Deploy') { 
        steps {
            deploy adapters: [tomcat9(credentialsId: '46079178-7ad2-46cd-bbfd-f6914abd1e93', path: '', url: 'http://ec2-35-87-43-6.us-west-2.compute.amazonaws.com:8080/')], contextPath: null, war: '**/*.war'
           }
        }
    }
    
}    
