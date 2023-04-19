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
        stage('CI'){
            failFast true
            parallel {
                stage('Unit Test') {
                    steps {
                        echo "Unit Test Stage Skip..."
                    }
                }
                stage('Sonarqube代码检测') {
                    steps {
                        container('tools') {
                            withSonarQubeEnv('sonar') {
                                sh 'sonar-scanner -X'
                                sleep 3
                            }
                            script {
                                timeout(1) {
                                    def qg = waitForQualityGate('sonar')
                                    if (qg.status != 'OK') {
                                        error "未通过Sonarqube的代码质量阈检查，请及时修改！failure: ${qg.status}"
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
        stage('Maven构建项目') {
            steps {
                container('tools') {
                    sh 'mvn clean package -DskipTest'
                }
                echo 'Maven构建项目 - SUCCESS'
            }
        }
        stage('Docker制作自定义镜像') {
            steps {
                container('tools') {
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
                container('tools') {
                    sh '''docker login -u ${registry_user} -p ${registry_password} ${registry_addr}
                    docker tag my_app_pipeline:$tag ${registry_addr}/my_app_pipeline:$tag
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