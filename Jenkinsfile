properties([
  buildDiscarder(logRotator(numToKeepStr: '2'))
])
pipeline {
    agent any

    tools {
        jdk 'jdk19'
    }

    environment {
        SONAR_PROJECT_KEY = 'adservice'
        SONAR_TOKEN = credentials('sonar') // must match Jenkins credential ID
        DOCKER_IMAGE = 'priyaa95/adservice:latest'
    }

    stages {
        stage('Trivy FS Scan') {
            steps {
                sh '''
                    echo "🔍 Scanning workspace with Trivy..."
                    trivy fs --exit-code 0 --severity HIGH,CRITICAL .
                     trivy image --severity HIGH,CRITICAL --format html -o trivy-image-report.html priyaa95/adservice:latest || true
                '''
                archiveArtifacts artifacts: 'trivy-image-report.html', allowEmptyArchive: true
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                        chmod +x ./gradlew
                        ./gradlew sonar \
                          -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
       stage('Trivy Docker Image Scan') {
          steps {
             sh 'trivy image priyaa95/adservice:latest || true'
          }
       }
       stage('Nexus Publish') {
          steps {
             withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                sh './gradlew publish -PnexusUser=$NEXUS_USER -PnexusPassword=$NEXUS_PASS'
        }
    }
}

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                       // sh "docker push ${DOCKER_IMAGE}"
                        sh "docker push priyaa95/adservice:latest "
                    }
                }
            }
        }
    }
}

