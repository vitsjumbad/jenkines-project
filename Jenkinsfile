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

        /* ---------------- GUARDRAIL ---------------- */
        stage('Guardrail Check') {
            steps {
                script {
                    // Impossible state safety
                    if (!buildingTag() && params.ENV == 'prod') {
                        error "❌ PROD deployments must come from a RELEASE TAG"
                       }                                 
                }
            }
        }

        /* ---------------- VALIDATE ---------------- */
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
                buildingTag()
            }
            steps {
                input message: "Approve PROD deployment for tag ${env.TAG_NAME}", ok: "Deploy"
            }
        }

        /* ---------------- PROD ---------------- */
        stage('Deploy to PROD') {
            when {
                buildingTag()
            }
            steps {
                echo "🚀 HOTFIX DEPLOYMENT to PROD from TAG ${env.TAG_NAME}"
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS"
        }
    }
}
