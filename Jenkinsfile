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
        stage('sonar scan'){
            steps{
                script{
                    def scannerHome = tool 'sonar-8.1'
                    withSonarQubeEnv('sonar-server'){
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "This run Evey time"
        }
        success {
            echo "current build is Success"
            // slackSend / emailext go here (plugins)
        }
        failure {
            echo "❌ Build failed — check stage logs above."
            // emailext subject: "FAILED: ${JOB_NAME} #${BUILD_NUMBER}", to: 'you@example.com', body: '...'
        }
        // cleanup {
        //     cleanWs()          // wipe workspace (Workspace Cleanup plugin)
        // }
    }
}