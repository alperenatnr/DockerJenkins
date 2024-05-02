pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/alperenatnr/DockerJenkins.git']]
                )
                bat 'mvn clean install'
            }
        }
        stage('Stop and Remove Existing Container') {
                                             steps {
                                                 script {
                                                   // Varolan container'ı durdur ve sil
                                                            bat 'docker stop demo-container '
                                                            bat 'docker rm demo-container'
                                                        }
                                                   }
                                        }

        stage('Build docker image'){
            steps{
                script{
                    docker.build("demo/app:${env.BUILD_NUMBER}")
                }
            }
        }



        stage('Run Docker Container') {
                    steps {
                        script {
                            docker.image("demo/app:${env.BUILD_NUMBER}").run("-d -p 4444:8080 --name demo-container")
                        }
                    }
                }

    }

}
