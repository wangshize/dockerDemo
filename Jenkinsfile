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
                sh '''mv target/*.jar .
                    docker build -t my_app_pipeline:${tag} .
                    '''
                echo 'Docker制作自定义镜像 - SUCCESS'
            }
        }
        stage('Push镜像到Harbor') {
            steps {
                sh '''docker login -u ${harbor_username} -p ${harbor_password} ${harbor_addr}
                    docker tag my_app_pipeline:$tag ${harbor_addr}/repo/my_app_pipeline:$tag
                    docker push ${harbor_addr}/repo/my_app_pipeline:$tag
                    '''
                echo 'Push镜像到Harbor - SUCCESS'
            }
        }
        stage('Publisher Over SSH通知目标服务器拉取镜像') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'test', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'deploy.sh ${harbor_addr} repo my_app_pipeline ${tag} 8080', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                echo 'Publisher Over SSH通知目标服务器拉取镜像 - SUCCESS'
            }
        }
    }

}