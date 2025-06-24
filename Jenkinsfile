properties([
  buildDiscarder(logRotator(numToKeepStr: '2'))
])
pipeline {
    agent any
    environment {
        SONAR_PROJECT_KEY = 'adservice'
        SONARQUBE_SERVER = 'sonar'
        SONAR_TOKEN = credentials('sonar') // must match Jenkins credential ID
       
       
    }

    stages {
      //   stage('SonarQube Analysis') {
      //       steps {
      //           script {
      //               def scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
      //               withSonarQubeEnv("${SONARQUBE_SERVER}") {
      //               sh """
      //                   ${scannerHome}/bin/sonar-scanner \
      //                   -Dsonar.projectKey=productcatalogueservice \
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
                        sh "docker build -t priyaa95/loadgenerator:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push priyaa95/loadgenerator:latest"
                    }
                }
            }
        }
    }
}
