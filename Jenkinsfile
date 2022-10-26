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
    }
}