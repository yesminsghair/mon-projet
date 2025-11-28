pipeline { 
    agent any 
    options { timestamps() } 
    environment { 
        DOCKER_IMAGE = 'yesminsghair/monapp' 
        DOCKER_TAG = "build-${env.BUILD_NUMBER}" 
    } 
    stages { 
        stage('Clone Git') { 
            steps { 
                git url: 'https://github.com/yesminsghair/mon-projet.git', branch: 'main' 
            } 
        } 
        stage('Build Docker') { 
            steps { 
                bat "wsl docker build -t %DOCKER_IMAGE%:%DOCKER_TAG% ." 
            } 
        } 
        stage('Deploy') { 
            steps { 
                bat "wsl docker-compose up -d" 
                bat 'echo "Application: http://localhost:8080"' 
            } 
        } 
    } 
} 
