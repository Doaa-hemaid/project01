@Library('shared-library@main') _
pipeline {
    agent { label 'ivolve' }

    environment {
        DOCKER_IMAGE_BASE = 'doaahemaid01/my-app'
        IMAGE_TAG = "${env.BUILD_ID}-${new Date().format('yyyyMMddHHmmss')}"
        DOCKER_IMAGE = "${DOCKER_IMAGE_BASE}:${IMAGE_TAG}"
        MINIKUBE_IP = '192.168.49.2'
        NAMESPACE = 'prod'
        K8SCREDENTIALS = 'k8s-prod'
    }

    stages {
        stage('Build Image') {

           steps {
                echo 'Building...'
                dockerBuildAndPush(DOCKER_IMAGE,'docker-hub-credentials')
                }
            }
        

        stage('Clean Local Images') {
            steps {
                echo 'Cleaning up local Docker images...'
                cleanDockerImage (DOCKER_IMAGE)
            }
        }

        stage('Deploy to prod Namespace') {
            steps {
                echo 'Deploying...'
               deployK8sApplication(NAMESPACE, DOCKER_IMAGE, K8SCREDENTIALS, MINIKUBE_IP)
           
                }
            }
        }
    

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
