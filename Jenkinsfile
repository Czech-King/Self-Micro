properties([
  buildDiscarder(logRotator(numToKeepStr: '100'))
])
pipeline {
    agent any

    tools {
        go 'Go-1.21'
     }
pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t priyaa95/checkoutservice:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Images') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push priyaa95/checkoutservice:latest "
                    }
                }
            }
        }
    }
}
