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
                    withDockerRegistry(credentialsId: 'docker') {
                        sh 'docker build -t shivashankardev/mongospring:2.2 .'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker') {
                        sh 'docker push shivashankardev/mongospring:2.2'
                    }
                }
            }
        }
        stage('docker container creation') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker') {
                        sh 'docker run -d -p 8082:8080 --name springapptwo shivashankardev/mongospring:2.2'
                    }
                }
            }
        }
    }
}
