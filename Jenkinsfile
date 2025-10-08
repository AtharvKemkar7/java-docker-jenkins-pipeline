pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "atharvk77/java-docker-jenkins-pipeline"   // ✅ Docker Hub repo name
    }

    stages {
        stage('Checkout') {
            steps {
                // ✅ Checkout from main branch
                git branch: 'main', url: 'https://github.com/AtharvKemkar7/java-docker-jenkins-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // ✅ Windows Jenkins agent command
                    bat 'docker build -t %DOCKER_IMAGE%:latest .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // ✅ Safely use Jenkins credentials here
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat """
                            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                            docker push %DOCKER_IMAGE%:latest
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Docker image built and pushed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
