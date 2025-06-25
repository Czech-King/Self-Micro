pipeline {
    agent any

    stages {
        stage('Deploy To Kubernets') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS-1', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://FDAB82FE5F5C972F956EC3842A50D59D.gr7.us-east-1.eks.amazonaws.com']]) {
                    sh "kubectl apply -f deployment-service.yml"
                    
                }
            }
        }
        
        stage('verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS-1', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://FDAB82FE5F5C972F956EC3842A50D59D.gr7.us-east-1.eks.amazonaws.com']]) {
                    sh "kubectl apply -f ingress.yml"
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}
