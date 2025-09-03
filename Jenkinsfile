pipeline {
    agent any

    stages {
stage('Checkout') {
 
    steps {
           
                git branch: 'https://github.com/prakash-128/java-project-maven-new.git'

               
             
            }
        }


        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }
 

      

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f hotstar:v1 || true
                    docker build -t hotstar:v1 .
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f con8 || true
                    docker run -d --name con8 -p 9943:8080 hotstar:v1
                '''
            }
        }

    }
}
