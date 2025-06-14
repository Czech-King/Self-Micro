pipeline {
    agent any
    stage('Code Quality') {
  environment {
    SONAR_TOKEN = credentials('sonar') // Jenkins secret with your SonarQube auth token
  }
  steps {
    dir('src') { // Adjust if your csproj is elsewhere
      withSonarQubeEnv('sonar') {
        sh '''
          # Install the .NET SonarScanner tool if not present
          dotnet tool install --global dotnet-sonarscanner || true
          export PATH="$PATH:$HOME/.dotnet/tools"

          # Start SonarQube analysis
          dotnet sonarscanner begin \
            /k:"Micro" \
            /n:"Micro" \
            /d:sonar.login=$SONAR_TOKEN

          # Build the project (this is what Sonar analyzes)
          dotnet build cartservice.csproj

          # End SonarQube analysis and push to server
          dotnet sonarscanner end /d:sonar.login=$SONAR_TOKEN
        '''
      }
    }
    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t adijaiswal/adservice:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push adijaiswal/adservice:latest "
                    }
                }
            }
        }
    }
}
