pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    dir('src') {

                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t adijaiswal/cartservice:latest ."
                    }
                        }
                }
            }
        }
        
        stage('Push Docker Images') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push adijaiswal/cartservice:latest "
                    }
                }
            }
        }
    }
}
