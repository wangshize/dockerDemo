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
                docker {
                    image 'maven:3-alpine'
                    //相当于jenkins在使用docker启动maven容器的时候，使用docker run -v  /var/jenkins_home/appconfig/maven/.m2:/root/.m2
                    //这样下次maven容器启动后，就可以挂在到主机目录上的本地仓库
                    args '-v /var/jenkins_home/appconfig/maven/.m2:/root/.m2'
                }
            }
            steps {
                echo "Building"
                sh 'pwd && ls -alh'
                sh 'mvn -v'
                // /var/jenkins_home 是jenkins镜像的home目录，流水线能使用这个目录下的所有东西，
                //因此虽然jenkins和maven是两个不同的镜像,但是流水线启动的maven容器编译的时候依然可以使用jenkins镜像里的文件

                //当然由于maven的容器也是jenkins在宿主机上启动的一个容器,因此settings.xml也可以挂在宿主机上的一个目录
                //但是不推荐这么用，因为jenkins未来需要迁移到其他主机的时候，只需要把jenkins_home这个文件夹一起拷贝走就可以了，
                //否则还需要找到个其他各种配置，他们的文件是怎么挂载的。
                sh 'mvn clean package -s "/var/jenkins_home/appconfig/maven/settings.xml" -Dmaven.test.skip=true'
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