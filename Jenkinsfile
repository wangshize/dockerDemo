pipeline {
    agent any
    environment {
        GO = 'golang'
    }
    stages {
        stage('Checking...') {
            steps {
                echo "Checking"
                sh 'java --version'
                sh 'git --version'
                sh 'docker version'
//                 sh 'mvn -v'
            }
        }
        stage('Build...') {
            steps {
                echo "Building"
                sh 'pwd && ls -alh'
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