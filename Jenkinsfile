pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "mgelvoleo"
        APP_NAME = "git-ops-project"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages {
        stage('Clean up workspace'){
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){
            steps {
                script {
                    git branch: 'main',
                    url: 'https://github.com/mgelvoleo/git-ops-project.git',
                    credentialsId: 'github'
                }
            }
  
        }
        stage('Build Docker Image'){
            steps{
                script{
                docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }

        stage('Delete Docker images'){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Updating kubernetes deployment file'){
            steps {
                script {
                    sh """
                        cat deployment.yml
                        sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                        cat dployment.yml
                    """
                }
            }
        } 
    }
}
// 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