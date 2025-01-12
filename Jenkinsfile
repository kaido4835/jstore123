pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/kaido4835/jstore123.git' // URL репозитория
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                powershell """
                    if (-Not (Test-Path .git)) {
                        git clone ${REPO_URL} .
                    } else {
                        git fetch --all
                        git reset --hard origin/main
                    }
                """
                echo 'Repository cloned successfully!'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker containers...'
                powershell 'docker-compose build'
                echo 'Build completed successfully!'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests inside Docker container...'
                powershell 'docker exec flask_app pytest tests/'
                echo 'Tests executed successfully!'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                powershell 'docker-compose up -d'
                echo 'Application deployed successfully!'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished!'
        }
        failure {
            echo 'Pipeline failed! Please check the logs.'
        }
    }
}
