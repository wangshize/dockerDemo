pipeline {
    agent any
    environment {
        WS = "${WORKSPACE}"
        jobName = "${JOB_BASE_NAME}"
    }
    stages {
        stage('开始执行流水线') {
            steps {
                echo '开始执行流水线'
            }
        }
        stage('拉取git仓库代码') {
            steps {
                checkout scmGit(branches: [[name: '${tag}']], extensions: [], userRemoteConfigs: [[url: 'https://gitee.com/wangshize/dockerDemo.git']])
                echo '拉取git仓库代码 - SUCCESS'
            }
        }
        stage('Maven构建项目') {
            steps {
                sh '/var/jenkins_home/maven/bin/mvn clean package -DskipTest'
                echo 'Maven构建项目 - SUCCESS'
            }
        }
        stage('Sonarqube代码检测') {
            steps {
                sh '/var/jenkins_home/sonar-scanner/bin/sonar-scanner -Dsonar.sources=./ -Dsonar.projectname=my_app_pipeline -Dsonar.projectKey=my_app_pipeline -Dsonar.login=c414dcb9997cffa18589673efc49e30efc7e3e8f -Dsonar.java.binaries=./target/'
                echo 'Sonarqube代码检测 - SUCCESS'
            }
        }
        stage('Docker制作自定义镜像') {
            steps {
                echo 'Docker制作自定义镜像 - SUCCESS'
            }
        }
        stage('Push镜像到Harbor') {
            steps {
                echo 'Push镜像到Harbor - SUCCESS'
            }
        }
        stage('Publisher Over SSH通知目标服务器拉取镜像') {
            steps {
                echo 'Publisher Over SSH通知目标服务器拉取镜像 - SUCCESS'
            }
        }
    }

}