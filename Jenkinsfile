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

        stage('Docker push') {
            steps {
                script {
                    docker.withRegistry('933273154934.dkr.ecr.us-east-1.amazonaws.com/digitalhouse-devops', 'awsdvops') {
                        docker.image('digitalhouse-app').push('latest')
                    }
                }
            }
        }


        stage('Deploy image') {

            steps {
                echo 'Deploy para Desenvolvimento'

            }
        }




    }
}