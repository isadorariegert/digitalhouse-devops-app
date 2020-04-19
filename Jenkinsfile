pipeline {

    agent any
    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build container'){
            when {
                branch 'dev'
            }            
            steps {

                print "Environment will be : ${env.NODE_ENV}"
                script {
                      try {
                            docker.build("digitalhouse-app:${env.BUILD_ID}")
                        }
                      catch(e){
                        echo "Caught: ${e}"
                        currentBuild.result = 'FAILURE'
                        error "Build stage failed"
                      }
                      finally{

                      }
                }
            }
            when {
                branch 'prod'
            }            
            steps {

                print "Environment will be : ${env.NODE_ENV}"

                script {
                      try {
                            docker.build("digitalhouse-app:latest")
                        }
                      catch(e){
                        echo "Caught: ${e}"
                        currentBuild.result = 'FAILURE'
                        error "Build stage failed"
                      }
                      finally{

                      }
                }
            }

        }

        stage('Deploy') {
            when {
                branch 'dev'
            }
            steps {
                echo 'Deploy para Desenvolvimento'
            }
            when {
                branch 'prod'
            }
            steps {
                echo 'Deploying para Producao'
            }            
        }

    }
}