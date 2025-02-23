pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "haridtvt/pratices:latest"
        REPO_URL = "https://github.com/haridtvt/Pratices"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker stop nginx-container || true'
                sh 'docker rm nginx-container || true'
                sh 'docker run -d --name nginx-container -p 8080:80 $DOCKER_IMAGE'
            }
        }
    }

    post {
        success {
            echo '✅ Build, Push & Deploy completed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
