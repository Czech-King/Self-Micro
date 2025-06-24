properties([
  buildDiscarder(logRotator(numToKeepStr: '5'))
])

pipeline {
    agent any

    // environment {
    //     SONARQUBE_SERVER = 'sonar' // Must match the name in Jenkins -> Manage Jenkins -> Configure System
    //     SONAR_TOKEN = credentials('sonar') // Jenkins credential ID for SonarQube token
    // }

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

        // stage('Test & Coverage') {
        //     steps {
        //         sh 'go test -v -coverprofile=coverage.out ./...'
        //     }
        // }

      //   stage('SonarQube Analysis') {
      //       steps {
      //           script {
      //               def scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
      //               withSonarQubeEnv("${SONARQUBE_SERVER}") {
      //               sh """
      //                   ${scannerHome}/bin/sonar-scanner \
      //                   -Dsonar.projectKey=shippingservice \
      //                   -Dsonar.sources=. \
      //                   -Dsonar.go.coverage.reportPaths=coverage.out \
      //                   -Dsonar.login=${SONAR_TOKEN} 
                        
      //               """
      //           }
      //       }
      //   }
      // }

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t priyaa95/shippingservice:latest ."
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push priyaa95/shippingservice:latest"
                    }
                }
            }
        }
    }
}
