pipeline {

    agent any
    tools {
        maven 'maven-3.6.3'
    }

    stages {

        stage('Git Checkout'){

            steps{
                git 'https://github.com/VenkataGKrishna/demo-application.git'
            }
        }
        stage('Unit Testing'){

            steps{
                sh 'mvn test'
            }
        }
        stage('Integration Testing'){

            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('Maven Build'){

            steps{
                sh 'mvn clean install'
            }
        }
        stage('Static Code Analysis'){
            
            steps{
                script{
                  withSonarQubeEnv(credentialsId: 'SONARKEY') {
                   sh 'mvn clean package sonar:sonar'     
                  }
                }
            }
        }
        stage("Quality Gate Status"){

            steps{
                timeout(time: 1, unit: 'HOURS') {
                  waitForQualityGate abortPipeline: false, credentialsId: 'SONARKEY'
                }
            }
        }
        stage("Upload warfile to Nexus"){

            steps{
      
                script{
                     def pom = readMavenPom file: 'pom.xml'
                     def mavenRepo = readMavenPom.version.endsWith ("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"
                     nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'springboot', 
                            classifier: '', file: 'target/Uber.jar', 
                            type: 'jar'
                            ]
                        ], 
                        credentialsId: 'NEXUS_AUTH', 
                        groupId: 'com.example', 
                        nexusUrl: '18.216.93.133:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: mavenRepo, 
                        version: "${pom.version}"
                }
            }
        }
    }
}