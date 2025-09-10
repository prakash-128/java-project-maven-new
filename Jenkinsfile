pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'prakash128'   // 🔁 Replace with your Docker Hub username
        IMAGE_NAME     = 'java-maven-app'
        IMAGE_TAG      = 'latest'
        CONTAINER_NAME = 'java-maven-container'
        APP_PORT       = '8080'  // Adjust if your app uses a different port
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/prakash-128/java-project-maven-new.git', branch: 'master'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG || true
                    docker build -t $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    # Stop and remove old container if it exists
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true

                    # Run new container
                    docker run -d --name $CONTAINER_NAME -p $APP_PORT:8080 $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
