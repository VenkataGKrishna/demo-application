pipeline {

    agent any
    tools {
        maven 'maven-3.6.3'
    }

    stages {

        stage('GIT Checkout'){

            steps{
                git 'https://github.com/VenkataGKrishna/demo-application.git'
            }
        }
        stage('UNIT Testing'){

            steps{
                sh 'mvn test'
            }
        }
    }
}