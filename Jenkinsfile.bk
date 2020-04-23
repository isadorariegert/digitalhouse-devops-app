pipeline {

    agent any
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
                    
                    try {
                        app = docker.build("digitalhouse-app:${env.BUILD_ID}")
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

        stage('Test image') {
            steps {
                script {
                    app.withRun('-p 8030:3000') { c ->
                        sh "curl -i http://127.0.0.1:8030/"
                    }
               
                }
            }
        }

        stage('Deploy image') {

            when {
                branch 'prod'
            }
            steps {
                echo 'Deploy para Desenvolvimento'
                script {
                    app.push("latest")
                }
            }
        }

    }
}