pipeline {
    agent any

    parameters {
        choice(
            name: 'ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select environment'
        )
    }

    environment {
        APP_NAME = "jenkins-learning"
    }

    stages {

        stage('Guardrail Check') {
            steps {
                script {
                    if (params.ENV == 'prod' && !env.TAG_NAME) {
                        error "❌ PROD deployment is allowed only from a Git TAG"
                    }
                }
            }
        }

        stage('Validate') {
            steps {
                echo "App: ${APP_NAME}"
                echo "Branch: ${env.BRANCH_NAME}"
                echo "Tag: ${env.TAG_NAME}"
                echo "Target ENV: ${params.ENV}"
            }
        }

        /* ---------------- DEV ---------------- */
        stage('Deploy to DEV') {
            when {
                allOf {
                    not { buildingTag() }
                    expression { params.ENV == 'dev' }
                }
            }
            steps {
                echo "Deploying to DEV from feature branch"
            }
        }

        /* ---------------- QA ---------------- */
        stage('Deploy to QA') {
            when {
                allOf {
                    not { buildingTag() }
                    expression { params.ENV == 'qa' }
                }
            }
            steps {
                echo "Deploying to QA from feature branch"
            }
        }

        /* ---------------- PROD APPROVAL ---------------- */
        stage('Approval for PROD') {
            when {
                allOf {
                    buildingTag()
                    expression { params.ENV == 'prod' }
                }
            }
            steps {
                input message: "Approve PROD deployment for tag ${env.TAG_NAME}", ok: "Deploy"
            }
        }

        /* ---------------- PROD ---------------- */
        stage('Deploy to PROD') {
            when {
                allOf {
                    buildingTag()
                    expression { params.ENV == 'prod' }
                }
            }
            steps {
                echo "🚀 Deploying to PROD from TAG ${env.TAG_NAME}"
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS"
        }
    }
}
