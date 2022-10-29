pipeline {

    agent any
    tools {
        maven 'maven-3.6.3'
    }

    stages{

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
                     def mavenRepo = pom.version.endsWith ("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"
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
        stage('Docker Image Build'){

            steps{

                script{
                    sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID venkata1981/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID venkata1981/$JOB_NAME:latest'
                }

            }
        }
        stage('Docker Image Build'){

            steps{

                script{
                    withCredentials([string(credentialsId: 'DOCKERHUB_CRED', variable: 'Docker_HUB')]) {
                       sh 'docker login -u venkata1981 -p ${Docker_HUB}'             
                       sh 'docker image push venkata1981/$JOB_NAME:v1.$BUILD_ID'
                       sh 'docker image push venkata1981/$JOB_NAME:latest'
                    }
                }
            }
        }
    }
}