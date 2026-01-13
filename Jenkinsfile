pipeline {
    agent any

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select environment'
        )

        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run tests or not'
        )
    }

    environment {
        APP_NAME = "jenkins-learning"
    }

    stages {

        stage('Validate') {
            steps {
                echo "App: ${APP_NAME}"
                echo "Environment: ${params.ENV}"
                echo "Run tests: ${params.RUN_TESTS}"
                echo "Branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Test') {
            when {
                expression { params.RUN_TESTS == true }
            }
            steps {
                echo "Running tests"
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
