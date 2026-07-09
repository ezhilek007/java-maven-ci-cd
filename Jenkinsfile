pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/ezhilek007/java-maven-ci-cd.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                cp target/*.war /var/lib/tomcat10/webapps/java-webapp.war
                '''
            }
        }

    }

    post {
        success {
            echo "Deployment Successful"
        }
        failure {
            echo "Deployment Failed"
        }
    }
}
