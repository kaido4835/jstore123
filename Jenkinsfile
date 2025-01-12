pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/zuwzuw/J-store.git' // URL репозитория
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                // Используем PowerShell для клонирования репозитория
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
                echo 'Running tests...'
                powershell 'pytest tests/' // Убедитесь, что pytest установлен и путь указан верно
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
