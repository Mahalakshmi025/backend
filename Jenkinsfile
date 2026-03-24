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
        region = 'us-east-1'
        account_id = '128759047428'
        project = 'expense'
        environment = 'dev'
        component = 'backend'
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
                withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                    sh """ 
                        aws ecr get-login-password --region ${region}  | docker login --username AWS --password-stdin ${account_id}.dkr.ecr.us-east-1.amazonaws.com
                        docker build -t ${account_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${environment}/${component}:${appVersion} .
                        
                        docker images

                        docker push ${account_id}.dkr.ecr.us-east-1.amazonaws.com/${project}/${environment}/${component}:${appVersion}
                    """
                }
            }
        }
    }
}