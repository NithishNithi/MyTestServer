pipeline {
    agent any

    environment {
        compose_service_name = "react-jenkins-docker"
    }
    stages {
        stage('Checkout Source') {
            steps ([$class: 'WsCleanup']){
                ws("${workspace}") {
                    checkout scm
                }
            }
        }
        stage('Docker Comopse Build') {
            steps {
                ws("${workspace}"){
                    sh "docker compose build --no-cache ${compose_service_name}"
                }
            }
        }
        stage('Docker Comopse Up') {
            steps {
                ws("${workspace}"){
                    sh "docker compose up --no-deps -d ${compose_service_name}"
                }
            }
        }
    }
}