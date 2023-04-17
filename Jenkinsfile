pipeline {
    agent any

    environment {
        NEXUS_URL = 'http://nexus.example.com'
        NEXUS_CREDENTIALS_ID = 'nexus-credentials'
        GCR_PROJECT_ID = 'rising-timing-383506'
        GKE_CLUSTER_NAME = 'my-gke-cluster'
        GKE_NAMESPACE = 'my-gke-namespace'
        GKE_CREDENTIALS_ID = 'rising-timing-383506'
    }
    
    
    stages {       
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build and push Docker image to GCR') {
            steps {
                withCredentials([gcpServiceAccount(credentialsId: GKE_CREDENTIALS_ID, jsonKeyVariable: 'GCP_SA_KEY')]) {
                    sh "gcloud auth activate-service-account --key-file <(echo '$GCP_SA_KEY')"
                    sh "gcloud auth configure-docker --quiet"
                    sh "docker build -t gcr.io/${GCR_PROJECT_ID}/my-app:${BUILD_NUMBER} ."
                    sh "docker push gcr.io/${GCR_PROJECT_ID}/my-app:${BUILD_NUMBER}"
                }
            }
        }
    }
}
