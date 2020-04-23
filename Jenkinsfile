pipeline {

    agent none

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    triggers {
        cron('@daily')
    }

    stage("CI") {
        agent { label 'master' }

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
                        docker.build("digitalhouse-devops:latest")
                    }
                }
            }

            stage('Test image') {
                steps {
                    script {

                        docker.image("digitalhouse-devops:latest").withRun('-p 8030:3000') { c ->
                            sh 'docker ps'
                            sh 'sleep 10'
                            sh 'curl http://127.0.0.1:8030/api/v1/healthcheck'
                            
                        }
                
                    }
                }
            }

            stage('Docker push') {
                steps {
                    echo 'Push latest para AWS ECR'
                    script {
                        docker.withRegistry('https://933273154934.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:awsdvops') {
                            docker.image('digitalhouse-devops').push()
                        }
                    }
                }
            }
        }
    }

    stage("CD") {
        agent { label 'dev' }

        stages {
            stage('Deploy em Homologacao') {

                when {

                    branch 'dev'
                }

                steps {
                    echo 'Deploy para Desenvolvimento'
                    sh "hostname"
                    sh "docker run -d --name app1 nginx:latest"
                    sh "docker ps"
                }

            }
        }
    }
}