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


        stage('Deploy em Homologacao') {
            when {

                branch 'dev'
            }

            steps {
                echo 'Deploy para Desenvolvimento'

            }

        }

        stage('Deploy em Produção') {
            when {

                branch 'prod'
            }

            steps {
                echo 'Deploy para Desenvolvimento'

            }
        }



    }
}