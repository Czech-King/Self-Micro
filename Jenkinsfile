pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t priyaa95/adservice:latest ."
                    }
                }
            }
        }
        environment {
        SONAR_PROJECT_KEY = 'my-gradle-app'              // Your project key in SonarQube    
        SONAR_TOKEN = credentials('sonar')         // Jenkins credential ID for SonarQube token
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {  // Name as configured in Jenkins
                    sh './gradlew sonarqube -Dsonar.projectKey=$SONAR_PROJECT_KEY -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }

        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push priyaa95/adservice:latest "
                    }
                }
            }
        }
    }
}
