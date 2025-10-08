pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'your-dockerhub-creds-id'   // Jenkins credentials ID for Docker Hub
        DOCKER_IMAGE = 'prakash128/hotstar:v1'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/prakash-128/java-project-maven-new.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Ensure Docker is running
                    sh 'sudo systemctl start docker || true'
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} ."
                    sh "docker run -itd --name hotstar -p 8083:80 ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push ${DOCKER_IMAGE}
                            docker logout
                        """
                    }
                }
            }
        }
    }
}
