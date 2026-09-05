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
                        sh 'docker build -t shivashankardev/mongospring:1.9 .'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: '868f6ab9-4e0c-4756-989a-625894345dcf') {
                        sh 'docker push shivashankardev/mongospring:1.9'
                    }
                }
            }
        }

            stage('Container creation') {
            steps {
                script {
                    withDockerRegistry(credentialsId: '868f6ab9-4e0c-4756-989a-625894345dcf') {
                        sh 'docker run -d -p 8085:8080 --name two  shivashankardev/mongospring:1.9'
                        
                    }
                }
            }
        }
            stage('Container creation for springapplication') {
            steps {
                script {
                    withDockerRegistry(credentialsId: '868f6ab9-4e0c-4756-989a-625894345dcf') {
                        sh 'docker run -d -p 8086:8080 --name springfive  shivashankardev/mongospring:1.9'
                        
                    }
                }
            }
        }
    }
}
