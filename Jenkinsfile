pipeline {
    agent any
    tools {
    jdk 'jdk21'
}

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
JAVA_HOME=$JAVA_HOME PATH=$JAVA_HOME/bin:$PATH ./gradlew sonarqube \
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
