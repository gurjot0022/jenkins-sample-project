pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t jenkins-sample-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Stopping old container if running..."
                sh 'docker rm -f jenkins-container || true'

                echo "Running new container..."
                sh 'docker run -d -p 8081:80 --name jenkins-container jenkins-sample-app'
            }
        }

    }
}
