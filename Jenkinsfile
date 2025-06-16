properties([
  buildDiscarder(logRotator(numToKeepStr: '100'))
])

pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'sonar' // Replace with your SonarQube server name in Jenkins config
        SONAR_TOKEN = credentials('sonar') // Replace with your credential ID
    }

    tools {
        go 'Go-1.24'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'go mod tidy'
                sh 'go build -v -o checkoutservice .'
            }
        }

        stage('Test & Coverage') {
            steps {
                sh 'go test -v -coverprofile=coverage.out ./...'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh """
                        ${SONAR_RUNNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=checkoutservice \
                        -Dsonar.sources=. \
                        -Dsonar.go.coverage.reportPaths=coverage.out
                        -Dsonar.login=${SONAR_TOKEN} \
                        
                    """
                }
            }
        }
      }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh 'docker build -t priyaa95/checkoutservice:latest .'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh 'docker push priyaa95/checkoutservice:latest'
                    }
                }
            }
        }
    }
}
