pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        APP_NAME = 'dhananjay03/nodejs-demo-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${APP_NAME}:latest ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh 'echo \(DOCKERHUB_CREDENTIALS_PSW | docker login -u\)DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh "docker push ${APP_NAME}:latest"
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker stop nodejs-app || true'
                sh 'docker rm nodejs-app || true'
                sh "docker run -d -p 3000:3000 --name nodejs-app ${APP_NAME}:latest"
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
