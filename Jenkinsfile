pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'branch', url: 'https://github.com/Gopi-Kumar18/DevOps-CI-CD-Project.git'
            }
        }

        stage('Prepare Environment') {
            steps {
                withCredentials([file(credentialsId: 'my-dotEnvFile', variable: 'ENV_FILE')]) {
                    sh '''
                        mkdir -p backend
                        cp "$ENV_FILE" backend/.env
                    '''
                }
            }
        }

        stage('Build & Deploy') {
            steps {
                withEnv(["DOCKER_HUB_USERNAME=${gopikumar1}", "DOCKER_HUB_PASSWORD=${Gopi@dhub&?}"]) {
                    sh '''
                        docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD
                        docker compose down
                        docker compose up -d --build
                        docker compose push
                    '''
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout'
            cleanWs()
        }
    }
}