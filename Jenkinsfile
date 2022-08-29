pipeline {
    agent any
    environment {
        GO = 'golang'
    }
    stages {
        stage('Build...') {
            steps {
                echo "Building"
                sh 'printenv'
                sh 'echo $GO Build'
            }

        }
        stage('Test...') {
            steps {
                echo "Testing"
            }
        }
        stage('Deploy...') {
            steps {
                echo "Deploying"
            }
        }
    }

}