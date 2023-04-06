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
    }
}
// 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