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
                echo "Branch: ${env.BRANCH_NAME}"
                echo "Target ENV: ${params.ENV}"
            }
        }

        /* ---------------- DEV DEPLOY ---------------- */
        stage('Deploy to DEV') {
            when {
                allOf {
                    not { branch 'main' }
                    expression { params.ENV == 'dev' }
                }
            }
            steps {
                echo "Deploying to DEV from FEATURE branch"
            }
        }

        /* ---------------- QA DEPLOY ---------------- */
        stage('Deploy to QA') {
            when {
                allOf {
                    not { branch 'main' }
                    expression { params.ENV == 'qa' }
                }
            }
            steps {
                echo "Deploying to QA from FEATURE branch"
            }
        }

        /* ---------------- PROD APPROVAL ---------------- */
        stage('Approval for PROD') {
            when {
                allOf {
                    branch 'main'
                    expression { params.ENV == 'prod' }
                }
            }
            steps {
                input message: 'Approve PROD deployment?', ok: 'Deploy'
            }
        }

        /* ---------------- PROD DEPLOY ---------------- */
        stage('Deploy to PROD') {
            when {
                allOf {
                    branch 'main'
                    expression { params.ENV == 'prod' }
                }
            }
            steps {
                echo "🚀 Deploying to PROD from MAIN branch"
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS"
        }
    }
}
