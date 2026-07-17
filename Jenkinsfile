pipeline {
    agent {
        node { label "AGENT-1" }
    } 
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm') 
    }
    environment {
        PROJECT = 'roboshop'
        COMPONENT = 'catalogue'
        AWS_REGION = 'us-east-1'
    }
    stages {
        stage('Read Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    def appVersion = packageJson.version
                    echo "The application version is: ${appVersion}"
                    env.APP_VERSION = appVersion
                }
            }
        }
        stage('Install Dependancies'){
            steps{
                sh '''
                    npm install
                '''
            }
        }
        stage('unit test'){
            steps{
                sh'''
                    npm test
                '''
            }
        }
    }
}