pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-creds')
        DOCKER_IMAGE = "atharvkemkar7/java-app"   // ✅ your Docker Hub username
    }

    stages {
        stage('Checkout') {
            steps {
                // ✅ Checkout from main branch (not master)
                git branch: 'main', url: 'https://github.com/AtharvKemkar7/java-docker-jenkins-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // ✅ Use bat instead of sh if you are on Windows Jenkins
                    // If using Windows Jenkins agent:
                    bat 'docker build -t %DOCKER_IMAGE%:latest .'

                    // If using Linux Jenkins agent:
                    // sh 'docker build -t $DOCKER_IMAGE:latest .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // ✅ Login & push the image
                    bat """
                        echo %DOCKER_HUB_CREDENTIALS_PSW% | docker login -u %DOCKER_HUB_CREDENTIALS_USR% --password-stdin
                        docker push %DOCKER_IMAGE%:latest
                    """

                    // If Linux Jenkins agent, use this instead:
                    // sh '''
                    //     echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin
                    //     docker push $DOCKER_IMAGE:latest
                    // '''
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
