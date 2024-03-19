pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git(url: 'https://github.com/bindu0900/task.git')
            }
        }
        stage('Build Docker Image') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                script {
                    sh '''
                    docker build -t task .
                    docker tag task:latest 372943597804.dkr.ecr.us-east-1.amazonaws.com/task:latest
                    '''
                }
            }
        }
        stage('Push Docker Image') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                script {
                    sh '''
                  aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 372943597804.dkr.ecr.us-east-1.amazonaws.com
                   docker push 372943597804.dkr.ecr.us-east-1.amazonaws.com/task:latest                
    '''
                }
            }
        }
    }
}
