pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    environment {
        DEBUG = 'true'
        appVersion = '' // this will become global, we can use across pipeline
    }
    stages {
        stage('Read the Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "App Version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {

                sh 'npm install'  
            }
        }
        stage('Docker Build') {
            steps {
                sh """ 
                    docker build -t pmahalakshmi25/backend:${appVersion} .
                    docker images
                """
            }
        }
    }
}