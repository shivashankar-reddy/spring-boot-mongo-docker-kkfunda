pipeline {
    agent any

    tools {
        maven 'maven-3.9.11'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shivashankar-reddy/spring-boot-mongo-docker-kkfunda.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}

