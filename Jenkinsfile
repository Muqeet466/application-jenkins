pipeline {
    agent any
           
    tools {
        jdk 'jdk11'
        maven 'maven3'
         
    }  
    
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
       
    }
    
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/vikash-kumar01/mrdevops_javaapplication.git'
                }
            }
        }
        
         stage('Code Compile') {
            steps { 
                 bat "mvn clean compile"
            }
        }
        
         stage('Unit Tests') {
            steps { 
                 bat "mvn test"
            }
        }
        
        stage('Sonarqube Analysis') {
            steps { 
              bat "mvn clean package"
              bat ''' mvn sonar:sonar -Dsonar.url=http://localhost:9000/ -Dsonar.login=38ce444cdea4a6380d07a8ac955b7422681ac3c7 -Dsonar.projectName=Petclinic \
                -Dsonar.java.binaries=. \
                -Dsonar.projectKey=Petclinic '''
            }
        }
        
        
        stage('OWASP SCAN') {
            steps { 
                dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**dependecy-check-report.xml'
            }
        }
        
        stage('Build Artifact') {
            steps { 
                 bat "mvn clean install"
            }
        }
    }
}
