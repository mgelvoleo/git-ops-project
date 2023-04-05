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
        stage('Clean up workspace') {
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
    }
}
// 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