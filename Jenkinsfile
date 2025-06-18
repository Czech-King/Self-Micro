properties([
  buildDiscarder(logRotator(numToKeepStr: '2'))
])
pipeline {
    agent any
    environment {
        SONAR_PROJECT_KEY = 'adservice'
        SONAR_TOKEN = credentials('sonar') // must match Jenkins credential ID
       
    }

    stages {
        stage('SonarQube Analysis') {
           steps {
                withSonarQubeEnv('sonar') {
                    sh 'SonarScanner -Dsonar.login=$SONAR_TOKEN'

                }
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t priyaa95/emailservice:latest ."
                    }
                }
            }
        }

        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push priyaa95/emailservice:latest "
                    }
                }
            }
        }
    }
}
