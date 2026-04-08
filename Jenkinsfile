pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/aarshbh/cicd-experiment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t cicd-app .'
            }
        }

        stage('Testing') {
            steps {
                bat '''
                docker run -d --name test-container -p 8081:80 cicd-app
                timeout /t 5
                curl -f http://localhost:8081
                docker stop test-container
                docker rm test-container
                '''
            }
        }

        stage('Deploy to Dev') {
            steps {
                bat '''
                docker stop dev-container || echo no container
                docker rm dev-container || echo no container
                docker run -d --name dev-container -p 8080:80 cicd-app
                '''
            }
        }

        stage('Deploy to Prod') {
            steps {
                bat '''
                docker stop prod-container || echo no container
                docker rm prod-container || echo no container
                docker run -d --name prod-container -p 9090:80 cicd-app
                '''
            }
        }
    }
}