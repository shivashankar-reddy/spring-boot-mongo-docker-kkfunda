pipeline {
    agent any

    tools {
        maven 'maven-3.9.11' // This should match the Maven name in Jenkins Global Tool Configuration
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

        stage('SonarQube') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=spring-boot-mongo \
                        -Dsonar.projectName='Spring Boot Mongo Project' \
                    """
                }
            }
        }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'sonar') {
                        sh 'docker build -t shivashankardev/mongospring:2.0 .'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'sonar') {
                        sh 'docker push shivashankardev/mongospring:2.0'
                    }
                }
            }
        }
    }
}
