pipeline {

    agent any
    tools {
        maven 'maven'
    }
    stages {

        stage('GIT Checkout'){

            steps{
                git 'https://github.com/VenkataGKrishna/demo-application.git'
            }
        }
        stage('UNIT Testing'){

            steps{
                sh '$(maven) test'
            }
        }
    }
}