pipeline {
    agent any

    tools {
        jdk 'jdk19'
    }

    environment {
        SONAR_PROJECT_KEY = 'adservice'
        SONAR_TOKEN = credentials('sonar') // must match Jenkins credential ID
        DOCKER_IMAGE = 'priyaa95/adservice:latest'
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                        chmod +x ./gradlew
                        ./gradlew clean test jacocoTestReport sonarqube
                        ./gradlew sonarqube \
                          -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
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
