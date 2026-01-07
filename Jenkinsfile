pipeline {
    agent any

    environment {
        APP_NAME = "jenkins-learning"
        ENVIRONMENT = "dev"
    }

    stages {
        stage('Use Secret') {
            steps {
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'TOKEN')]) {
                    bat 'echo Token is $TOKEN'
                }
            }
        }
    }
}

