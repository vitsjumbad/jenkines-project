pipeline {
    agent any

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select deployment environment'
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

        /* ---------------- GUARDRAIL ---------------- */
        stage('Guardrail Check') {
            steps {
                script {

            // PROD must come ONLY from TAG
                    if (params.ENV == 'prod' && !buildingTag()) {
                        error "❌ PROD deployment is allowed ONLY from Git TAGS"
                    }

            // TAG builds must deploy ONLY to PROD
                    if (buildingTag() && params.ENV != 'prod') {
                        error "❌ Git TAGS can deploy ONLY to PROD"
                    }
                }
            }
        }

        /* ---------------- VALIDATION ---------------- */
        stage('Validate') {
            steps {
                echo "App Name     : ${APP_NAME}"
                echo "Branch Name  : ${env.BRANCH_NAME}"
                echo "Tag Name     : ${env.TAG_NAME ?: 'N/A'}"
                echo "Target ENV   : ${params.ENV}"
                echo "Run Tests    : ${params.RUN_TESTS}"
            }
        }

        /* ---------------- TESTS ---------------- */
        stage('Test') {
            when {
                expression { params.RUN_TESTS }
            }
            steps {
                echo "Running tests..."
                bat 'type message.txt'
            }
        }

        /* ---------------- DEV DEPLOY ---------------- */
        stage('Deploy to DEV') {
            when {
                allOf {
                    not { buildingTag() }
                    expression { params.ENV == 'dev' }
                }
            }
            steps {
                echo "🚧 Deploying to DEV from feature branch"
            }
        }

        /* ---------------- QA DEPLOY ---------------- */
        stage('Deploy to QA') {
            when {
                allOf {
                    not { buildingTag() }
                    expression { params.ENV == 'qa' }
                }
            }
            steps {
                echo "🧪 Deploying to QA from feature branch"
            }
        }

        /* ---------------- PROD APPROVAL ---------------- */
        stage('Manual Approval for PROD') {
            when {
                allOf {
                    buildingTag()
                    expression { params.ENV == 'prod' }
                }
            }
            steps {
                input message: "Approve PROD deployment for release ${env.TAG_NAME}",
                      ok: "Deploy"
            }
        }

        /* ---------------- PROD DEPLOY ---------------- */
        stage('Deploy to PROD') {
            when {
                allOf {
                    buildingTag()
                    expression { params.ENV == 'prod' }
                }
            }
            steps {
                echo "🚀 Deploying to PROD from RELEASE TAG ${env.TAG_NAME}"
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}
