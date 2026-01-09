pipeline {
    agent any

    environment {
        APP_NAME = "jenkins-learning"
        ENVIRONMENT = "dev"
    }

    stages {

        stage('Validate') {
            steps {
                echo "App: ${APP_NAME}"
                echo "Env: ${ENVIRONMENT}"
            }
        }

        stage('Use Secret') {
            steps {
                withCredentials([
                    string(credentialsId: 'GITHUB_TOKEN', variable: 'TOKEN')
                ]) {
                    bat 'echo Token is available'
                }
            }
        }

        stage('Test') {
            steps {
                bat 'type message.txt'
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS"
        }
        failure {
            echo "Pipeline FAILED"
        }
    }
}
