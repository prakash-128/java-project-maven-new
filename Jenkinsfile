pipeline {
    agent any

    environment {
        IMAGE_NAME = 'hotstar'
        IMAGE_TAG = 'v1'
        DOCKERHUB_USER = 'your-dockerhub-username'
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
        stage('Deploy to Docker Swarm') {
            steps {
                sh '''
                    docker service rm hotstar-service || true

                    docker service create \
                        --name hotstar-service \
                        --publish 9943:8080 \
                        $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
