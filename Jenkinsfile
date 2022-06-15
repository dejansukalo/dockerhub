pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dejansukalo-dockerhub')
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t dejansukalo/ds-alpine:latest .'
            }
        }
        stage('Start container') {
            steps {
                sh 'docker-compose up'
                sh 'docker-compose ps'
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push dejansukalo/ds-alpine:latest'
            }
        }
    }
    post {
        always {
            sh 'docker-compose down --remove-orphans -v'
            sh 'docker ps'
            sh 'docker logout'
        }
    }
}