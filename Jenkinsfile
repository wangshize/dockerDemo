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
                sh 'pwd && ls -alh'
            }
        }
        stage('Build...') {
            agent {
                docker { image 'maven:3-alpine' }
            }
            steps {
                echo "Building"
                sh 'pwd && ls -alh'
                sh 'mvn -v'
                sh 'mvn clean package -Dmaven.test.skip=true'
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