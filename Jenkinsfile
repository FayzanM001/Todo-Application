pipeline {
    agent any

    environment {
        APP_NAME = 'todo-3tier-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out latest code from GitHub...'
                checkout scm
            }
        }

        stage('Lint & Validate Compose') {
            steps {
                script {
                    echo 'Validating docker-compose configuration...'
                    sh 'docker compose config || docker-compose config'
                }
            }
        }

        stage('Deploy 3-Tier Application') {
            steps {
                script {
                    echo 'Stopping old containers and building new 3-tier stack...'
                    sh 'docker compose down --remove-orphans || docker-compose down --remove-orphans'
                    sh 'docker compose up -d --build || docker-compose up -d --build'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying active 3-tier containers...'
                    sh 'sleep 15'
                    sh 'docker ps'
                }
            }
        }
    }

    post {
        success {
            echo '3-Tier To-Do Application successfully deployed!'
        }
        failure {
            echo 'Pipeline failed. Check Jenkins console logs for details.'
        }
    }
}