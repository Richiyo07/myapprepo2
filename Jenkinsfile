pipeline {
    
   agent any
   
   tools {
    maven 'maven3' 
   }
    stages {
        
       
        stage('Checkout') {
       steps {
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '995ca1ca-56ce-4d0b-ba42-71dfe0969ba7', url: 'https://github.com/Richiyo07/myapprepo2']])
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
      nexusUrl: 'ec2-52-91-48-20.compute-1.amazonaws.com:8081',
      groupId: 'myGroupId',
      version: '1.0-SNAPSHOT',
      repository: 'maven-snapshots',
      credentialsId: '7a8a7a31-238c-4ea9-90cf-b76f2483cd55',
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
            deploy adapters: [tomcat9(credentialsId: '4aa321a7-54ea-490f-83cf-97f0e0bc6604', path: '', url: 'http://ec2-44-201-204-127.compute-1.amazonaws.com:8080')], contextPath: null, war: '**/*.war'
           }
        }
    }
    
}    
