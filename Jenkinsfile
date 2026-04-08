pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/your-username/cicd-experiment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t cicd-app .'
                }
            }
        }

        stage('Testing') {
            steps {
                script {
                    sh '''
                    docker run -d --name test-container -p 8081:80 cicd-app
                    timeout /t 5
                    curl -f http://localhost:8081
                    docker stop test-container
                    docker rm test-container
                    '''
                }
            }
        }

        stage('Deploy to Dev') {
            steps {
                script {
                    sh '''
                    docker stop dev-container || true
                    docker rm dev-container || true
                    docker run -d --name dev-container -p 8080:80 cicd-app
                    '''
                }
            }
        }

        stage('Deploy to Prod') {
            steps {
                script {
                    sh '''
                    docker stop prod-container || true
                    docker rm prod-container || true
                    docker run -d --name prod-container -p 9090:80 cicd-app
                    '''
                }
            }
        }
    }
}