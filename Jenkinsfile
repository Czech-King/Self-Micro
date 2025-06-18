pipeline {
    agent any
    tools {
        nodejs 'Node20'  // Match this with the name defined in Jenkins > Global Tool Configuration
    }


    stages {
        stage('Install dependencies') {
            steps {
                sh 'node -v'       // Should show v20.2.0
                sh 'npm -v'
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        // Additional stages...
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
    
