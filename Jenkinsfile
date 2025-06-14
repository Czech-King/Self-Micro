pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    dir('src') {

                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t priyaa95/cartservice:latest ."
                    }
                        }
                }
            }
        }
        
        stage('Push Docker Images') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push priyaa95/cartservice:latest "
                    }
                }
            }
        }
    }
}
