pipeline {
    agent any

    environment {
        APP_NAME = "jenkins-learning"
        ENVIRONMENT = "dev"
    }

    stages {
        stage('Print Env') {
            steps {
                echo "App Name: ${APP_NAME}"
                echo "Environment: ${ENVIRONMENT}"
            }
        }

        stage('Read File') {
            steps {
                bat 'type message.txt'
            }
        }
    }
}

