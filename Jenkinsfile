pipeline {
    agent any

    environment {
        DOCKERHUB_USER = 'prakash'   // 🔁 Replace with your Docker Hub username
        IMAGE_NAME = 'java-maven-app'                // 🔁 Replace with your desired image name
        IMAGE_TAG = 'latest'                         // 🔁 Use a version/tag as needed
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

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'prakash128', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker push $DOCKERHUB_USER/$IMAGE_NAME:$IMAGE_TAG
                    '''
                }
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
