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
                echo "Branch: ${env.BRANCH_NAME}"
            }
        }

        stage('PR Quality Gate') {
            when {
                not {
                    branch 'main'
                }
            }
            steps {
                echo "Running PR quality checks on feature branch"
            }
        }

        stage('Use Secret') {
            when {
                branch 'main'
            }
            steps {
                withCredentials([
                    string(credentialsId: 'GITHUB_TOKEN', variable: 'TOKEN')
                ]) {
                    echo "Secret accessed safely on main branch"
                }
            }
        }

        stage('Test') {
            when {
                branch 'main'
            }
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
