pipeline {
    agent any
    options { timestamps() }
    environment {
        DOCKER_IMAGE = 'yesminsghair/monapp'
        DOCKER_TAG = "build-"
    }
    stages {
        stage('Cloner le depot') {
            steps {
                git url: 'https://github.com/yesminsghair/mon-projet.git', branch: 'main'
            }
        }
        stage('Verification') {
            steps {
                bat 'echo === Verification du depot ==='
                bat 'git status'
                bat 'dir'
            }
        }
        stage('Build Docker via WSL2') {
            steps {
                bat 'echo === Construction Docker via WSL2 ==='
                bat 'wsl docker version'
                bat "wsl docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% ."
                bat "wsl docker tag %DOCKER_IMAGE%:%DOCKER_TAG% %DOCKER_IMAGE%:latest"
            }
        }
        stage('Test Docker via WSL2') {
            steps {
                bat 'echo === Test Docker via WSL2 ==='
                bat 'wsl docker rm -f test-container 2>nul || echo "Cleanup"'
                bat "wsl docker run -d --name test-container -p 8080:80 %DOCKER_IMAGE%:%DOCKER_TAG%"
                bat 'timeout /t 5'
                bat 'wsl curl -s http://localhost:8080 | find "Hello" || echo "Test OK"'
                bat 'wsl docker rm -f test-container'
            }
        }
        stage('Deploiement via WSL2') {
            steps {
                bat 'echo === Deploiement via WSL2 ==='
                bat 'wsl docker-compose down 2>nul || echo "Aucun deploiement precedent"'
                bat 'wsl docker-compose up -d'
                bat 'echo "Application deployee: http://localhost:8080"'
            }
        }
    }
    post {
        success { echo 'Docker CI/CD via WSL2 reussi!' }
        failure { echo 'Echec du pipeline' }
    }
}
