pipeline {
    agent { label 'jnlp-slave'}
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
                container('tools') {
                    checkout scmGit(branches: [[name: '${branch}']], extensions: [], userRemoteConfigs: [[url: 'https://gitee.com/wangshize/dockerDemo.git']])
                }
                echo '拉取git仓库代码 - SUCCESS'
            }
        }
        stage('Maven构建项目') {
            steps {
                container('maven') {
                    sh 'mvn clean package -DskipTest'
                }
                echo 'Maven构建项目 - SUCCESS'
            }
        }
        stage('Docker制作自定义镜像') {
            steps {
                container('docker') {
                    sh '''mv target/*.jar .
                    docker ps
                    docker build -t my_app_pipeline:${tag} .
                    '''
                }
                echo 'Docker制作自定义镜像 - SUCCESS'
            }
        }
        stage('Push镜像到Registry') {
            steps {
                container('docker') {
                    sh '''docker tag my_app_pipeline:$tag ${registry_addr}/my_app_pipeline:$tag
                    docker push ${registry_addr}/my_app_pipeline:$tag
                    '''
                }
                echo 'Push镜像到Registry - SUCCESS'
            }
        }
        stage('Publisher Over SSH通知目标服务器拉取镜像') {
            steps {
                echo 'Publisher Over SSH通知目标服务器拉取镜像 - SUCCESS'
            }
        }
    }

}