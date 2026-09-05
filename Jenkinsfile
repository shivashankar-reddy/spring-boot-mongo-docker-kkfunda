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
                    withDockerRegistry(credentialsId: '868f6ab9-4e0c-4756-989a-625894345dcf') {
                        sh 'docker build -t shivashankardev/mongospring:2.0 .'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: '868f6ab9-4e0c-4756-989a-625894345dcf') {
                        sh 'docker push shivashankardev/mongospring:2.0'
                    }
                }
            }
        }

            stage('Container creation') {
            steps {
                script {
                    withDockerRegistry(credentialsId: '868f6ab9-4e0c-4756-989a-625894345dcf') {
                        sh 'docker run -d -p 8088:8080 --name three  shivashankardev/mongospring:2.0'
                        
                    }
                }
            }
        }
            stage('Container creation for springapplication') {
            steps {
                script {
                    withDockerRegistry(credentialsId: '868f6ab9-4e0c-4756-989a-625894345dcf') {
                        sh 'docker run -d -p 8087:8080 --name springten  shivashankardev/mongospring:2.0'
                        
                    }
                }
            }
        }
    }
}
