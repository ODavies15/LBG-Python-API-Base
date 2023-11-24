pipeline {

    agent any

    environment {

        GCR_CREDENTIALS_ID = 'oli-jenkins-gcr-sv'

        IMAGE_NAME = 'oli-week3-project'

        GCR_URL = 'gcr.io/lbg-mea-15'

        PROJECT_ID = ' lbg-mea-15 '

        CLUSTER_NAME = 'oli-cluster'

        LOCATION = 'europe-west2-c'

        CREDENTIALS_ID = 'a48e06ee-febe-4ccb-931b-fe10eac7499c'

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

                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kubernetes/deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])

                }

            }

        }

    }

}