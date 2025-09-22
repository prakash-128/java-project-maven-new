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

        
    }
}
