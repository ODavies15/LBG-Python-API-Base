pipeline {

    agent any

    environment {

        GCR_CREDENTIALS_ID = 'oli-jenkins-gcr-sv'

        IMAGE_NAME = 'oli-week3-project'

        GCR_URL = 'gcr.io/lbg-mea-15'

        PROJECT_ID = ' lbg-mea-15 '

        CLUSTER_NAME = 'oli-cluster'

        LOCATION = 'europe-west2-c'

        CREDENTIALS_ID = '3f817c18-0d27-4f6e-b7c1-aa6527a07ef1'

    }

    stages {

        stage('Build and Push to GCR') {

            steps {

                script {

                    // Authenticate with Google Cloud

                    withCredentials([file(credentialsId: GCR_CREDENTIALS_ID, variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {

                        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'

                    }

                    // Configure Docker to use gcloud as a credential helper

                    sh 'gcloud auth configure-docker --quiet'

                    // Build the Docker image

                    sh "docker build -t ${GCR_URL}/${IMAGE_NAME}:latest ."

                    // Push the Docker image to GCR

                    sh "docker push ${GCR_URL}/${IMAGE_NAME}:latest"

                }

            }

        }

        stage('Deploy to GKE') {

            steps {

                script {

                    // Deploy to GKE using Jenkins Kubernetes Engine Plugin
                    sh'gcloud config set account oli-jenkins@lbg-mea-15.iam.gserviceaccount.com'
                    sh'kubectl apply -f kubernetes/deployment.yaml'
                }

            }

        }

    }

}