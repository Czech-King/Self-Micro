pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = 'my-gradle-app'
        SONAR_TOKEN = credentials('sonar')
        DOCKER_IMAGE = 'priyaa95/adservice:latest'
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''chmod +x ./gradlew
./gradlew sonarqube \
  -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
  -Dsonar.login=${SONAR_TOKEN}'''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
    }
}
