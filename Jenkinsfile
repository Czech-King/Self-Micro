properties([
  buildDiscarder(logRotator(numToKeepStr: '2', daysToKeepStr: '0'))
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
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                        chmod +x ./gradlew
                        ./gradlew sonar.login \
                        sh 'cat build/sonar/report-task.txt || echo "report-task.txt not found"'
                          -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
        stage('Quality Gate') {
           steps {
              timeout(time: 2, unit: 'MINUTES') {
                  waitForQualityGate abortPipeline: true
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
