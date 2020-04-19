pipeline {

    agent any
    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Test'){
            when {
                branch 'dev'
            }            
            steps {

                print "Environment will be : ${env.NODE_ENV}"

              
            }
        }

        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Deploy para a DEV') {
            when {
                branch 'dev'
            }
            steps {
                echo 'Deploy para Desenvolvimento'
            }
        }

        stage('Deploy para a Producao') {
            when {
                branch 'prod'
            }
            steps {
                echo 'Deploying para Producao'
            }
        }
    }
}