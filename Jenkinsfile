pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    environment {
        GIT_URL = 'https://github.com/ezhilek007/java-maven-ci-cd.git'
        GIT_BRANCH = 'main'

        TOMCAT_URL = 'http://localhost:8085'
        APP_NAME = 'java-webapp'

        TOMCAT_CREDENTIALS = credentials('tomcat-user')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${GIT_BRANCH}", url: "${GIT_URL}"
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
                sh 'mvn clean package'
            }
        }

        stage('Verify WAR') {
            steps {
                sh '''
                ls -lh target
                test -f target/java-webapp.war
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                curl --fail \
                --user ${TOMCAT_CREDENTIALS_USR}:${TOMCAT_CREDENTIALS_PSW} \
                --upload-file target/java-webapp.war \
                "${TOMCAT_URL}/manager/text/deploy?path=/${APP_NAME}&update=true"
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                curl --fail \
                --user ${TOMCAT_CREDENTIALS_USR}:${TOMCAT_CREDENTIALS_PSW} \
                ${TOMCAT_URL}/manager/text/list
                '''
            }
        }
    }

    post {

        success {
            echo "Application deployed successfully."
        }

        failure {
            echo "Deployment failed."
        }

        always {
            archiveArtifacts artifacts: 'target/*.war', fingerprint: true
        }
    }
}
