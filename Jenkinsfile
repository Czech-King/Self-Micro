pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t mpriyaaa92@gmail.com/checkoutservice:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Images') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push adijaiswal/checkoutservice:latest "
                    }
                }
            }
        }
    }
}
