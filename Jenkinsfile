pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/JAWERIYAKHAN26/jenkins-sample-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh 'docker build -t jenkins-sample-app .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo 'Stopping old containers if any...'
                    sh '''
                    @echo off
                    setlocal
                    set containers=
                    for /F "tokens=*" %%i in ('docker ps -q --filter "ancestor=jenkins-sample-app"') do set containers=%%i
                    if defined containers (
                        echo Stopping container %containers%
                        docker stop %containers%
                        docker rm %containers%
                    ) else (
                        echo No running containers to stop
                    )

                    REM Free port 8081 if occupied
                    for /f "tokens=5" %%p in ('netstat -ano ^| findstr :8081') do (
                        echo Killing process on port 8081: PID %%p
                        taskkill /PID %%p /F
                    )

                    REM Run new container
                    docker run -d -p 8081:80 jenkins-sample-app
                    endlocal
                    '''
                }
            }
        }
    }
}
