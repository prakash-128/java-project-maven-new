pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'your-dockerhub-creds-id'   // Jenkins credentials ID for Docker Hub
        DOCKER_IMAGE = 'your-dockerhub-username/hotstar:v1'
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

        stage('Deploy to Docker Swarm') {
            steps {
                script {
                    sh """
                        docker service update --image ${DOCKER_IMAGE} hotstar_service || \
                        docker service create --name hotstar_service -p 8080:8080 ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }
}
