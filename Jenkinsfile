pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo '🔧 Starting Build...'
                sh 'npm install'
                sh 'bash genproto.sh'
                sh 'npm run lint || true'
                echo '✅ Build Complete.'
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t priyaa95/currencyservice:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push priyaa95/currencyservice:latest "
                    }
                }
            }
        }
    }
}
