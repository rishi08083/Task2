pipeline {
    agent any

    environment {
        IMAGE_NAME = "task2-demo"
        CONTAINER_NAME = "task2-container"
        PORT = "3100"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                git branch: 'main', url: 'https://github.com/rishi08083/Task2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                bat 'docker build -t %IMAGE_NAME%:latest .'
            }
        }

        stage('Test (Dummy)') {
            steps {
                echo 'Running dummy test...'
                bat 'echo All tests passed successfully!'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container...'
                bat 'docker rm -f %CONTAINER_NAME% || exit 0'
                bat 'docker run -d --name %CONTAINER_NAME% -p %PORT%:%PORT% %IMAGE_NAME%:latest'
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Build Failed!'
        }
    }
}
