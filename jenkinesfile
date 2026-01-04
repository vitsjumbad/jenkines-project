pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Code is already checked out by Jenkins'
            }
        }

        stage('Read File') {
            steps {
                bat 'type message.txt'
            }
        }
    }
}
