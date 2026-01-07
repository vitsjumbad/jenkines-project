pipeline {
    agent any

    environment {
        APP_NAME = "jenkins-learning"
        ENVIRONMENT = "dev"
    }

    stages {

        stage('Validate') {
            steps {
                echo "Validating environment"
                echo "App: ${APP_NAME}"
                echo "Env: ${ENVIRONMENT}"
            }
        }

        stage('Build') {
            steps {
                echo "Build stage started"
                bat 'echo Building application...'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests"
                bat 'type message.txt'
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully ✅"
        }
        failure {
            echo "Pipeline failed ❌"
        }
        always {
            echo "Pipeline finished (success or failure)"
        }
    }
}

