pipeline {

    agent none

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    triggers {
        cron('@daily')
    }

    stages {

        stage('Clone repository') {
            agent { label 'master' }
            steps {
                checkout scm
            }
        }
        stage('Build image'){   
            agent { label 'master' }       
            steps {
                script {
                    print "Environment will be : ${env.NODE_ENV}"
                    docker.build("digitalhouse-devops:latest")
                }
            }
        }

        stage('Test image') {
            agent { label 'master' }
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
            agent { label 'master' }
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

            agent { label 'dev' }

            when {

                branch 'dev'
            }

            steps {
                echo 'Deploy para Desenvolvimento'
                sh "docker run -d --name app1 nginx:latest"
//                sh "docker run -d --name app1 -p 8030:3000 digitalhouse-devops:latest"
                sh "docker ps"
            }

        }





    }
}