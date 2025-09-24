pipeline {

    agent {
        label 'docker-agent'
    }

    
    environment {
        
        DOCKERHUB_USER = 'abhi479'
        IMAGE_NAME = "${DOCKERHUB_USER}/my-node-app"
    }

    
    stages {
        stage('Checkout') {
            steps {
                
                checkout scm
            }
        }

        stage('Build Docker Image') {
            when { branch 'master' }
            steps {
                script {
                    echo "Building Docker image: ${IMAGE_NAME}:latest"
                    
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Publish Docker Image') {
            when { branch 'master' }
            steps {
                script {
                    echo "Publishing Docker image to Docker Hub..."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                        
                        echo "$DOCKER_PASS" | sh "docker login -u '${DOCKER_USER}' --password-stdin"

                        
                        sh "docker push ${IMAGE_NAME}:latest"

                        
                        sh "docker logout"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
            
            sh "docker rmi ${IMAGE_NAME}:latest || true"
        }
    }
}
