pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/ezhilek007/java-maven-ci-cd.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t java-webapp:v1 .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f java-container || true
                docker run -d \
                --name java-container \
                -p 8080:8080 \
                java-webapp:v1
                '''
            }
        }

    }
}
