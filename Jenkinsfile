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
                  withSonarQubeEnv(credentialsId: 'SONAR_KEY') {
                   sh 'mvn clean package sonar:sonar'     
                  }
                }
            }
        }
        stage("Quality Gate Status"){

            steps{
                script{
                  waitForQualityGate abortPipeline: false, credentialsId: 'SONAR_KEY'
                }
            } 
        }
    }
}