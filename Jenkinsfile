pipeline {

    agent any

    tools {
        maven 'Maven'
        jdk 'JDK11'
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/yourusername/java-webapp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war'
            }
        }

    }

}
