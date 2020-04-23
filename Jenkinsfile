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
                    app = docker.build("digitalhouse-app:${env.BUILD_ID}")
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    app.withRun('-p 8030:3000') { c ->
                        sh 'curl -s -w "%{http_code}\\\n" -o /dev/null http://127.0.0.1:8030/api/v1/healthcheck'
                        
                    }
               
                }
            }
        }

        stage('Deploy image') {

            steps {
                echo 'Tag latest da imagem depois do teste'
                sh 'docker tag "digitalhouse-app:${env.BUILD_ID}" digitalhouse-app:latest'
                echo 'Deploy para Desenvolvimento'

            }
        }

    }
}