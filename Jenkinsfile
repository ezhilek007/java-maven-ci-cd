pipeline {

    agent any

    tools {

        maven 'Maven3'

        jdk 'JDK21'
    }

    environment {

        TOMCAT_URL = 'http://52.66.227.122:8080'

        TOMCAT_USER = credentials('tomcat-user')

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

                sh 'mvn clean compile'

            }

        }

        stage('Test') {

            steps {

                sh 'mvn test'

            }

        }

        stage('Package') {

            steps {

                sh 'mvn package'

            }

        }

        stage('Deploy') {

            steps {

                sh '''
                curl -u $TOMCAT_USER \
                --upload-file target/java-webapp.war \
                "$TOMCAT_URL/manager/text/deploy?path=/sample&update=true"
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
