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
    
     stage('PR Check') {
    when {
        expression {
            return env.CHANGE_ID != null
        }
    }
    steps {
        echo "Running PR checks"
        echo "PR ID: ${env.CHANGE_ID}"
        echo "From: ${env.CHANGE_BRANCH}"
        echo "To: ${env.CHANGE_TARGET}"
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
                bat 'echo Secret available only on main'
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
