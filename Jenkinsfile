pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = 'my-gradle-app'               // SonarQube project key
        SONAR_TOKEN = credentials('sonar')                // SonarQube token from Jenkins credentials
       
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh "./gradlew sonarqube " +
                       "-Dsonar.projectKey=${SONAR_PROJECT_KEY} " +
                       "-Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                      //  sh "docker push ${DOCKER_IMAGE}"
                        sh "docker push priyaa95/adervice:latest "
                    }
                }
            }
        }
    }
}
