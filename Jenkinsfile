pipeline {

    agent { label 'master' }

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    triggers {
        cron('@daily')
    }

    stages {

        stage('Clone repository') {
            steps {
                checkout scm
            }
        }
        stage('Build image'){          
            steps {
                script {
                    print "Environment will be : ${env.NODE_ENV}"
                    docker.build("digitalhouse-app:${env.BUILD_ID}")
                }
            }
        }

        stage('Test image') {
            steps {
                script {

                    docker.image("digitalhouse-app:${env.BUILD_ID}").withRun('-p 8030:3000') { c ->
                        sh 'docker ps'
                        sh 'curl http://127.0.0.1:8030/api/v1/healthcheck'
                        
                    }
               
                }
            }
        }

        stage('Deploy image') {

            steps {
                echo 'Tag latest da imagem depois do teste'
                docker.tag
                sh "docker tag digitalhouse-app:${env.BUILD_ID} digitalhouse-app:latest"
                echo 'Deploy para Desenvolvimento'

            }
        }

    }
}