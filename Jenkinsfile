pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build .'
            }
        }
        stage('Publish in joylandbriefing') {
            environment {
                DOCKER_CREDENTIALS_ID = "docker.hub.credential.id"
                DOCKER_IMAGE_NAME = "joylandbriefing/ci-cd"
            }
            stages {
                stage('Publish image from pull request') {
                    when {
                        changeRequest target: 'main'
                    }
                    steps {
                        withDockerRegistry(credentialsId: "${DOCKER_CREDENTIALS_ID}", url: "") {
                            sh 'docker build --tag "${DOCKER_IMAGE_NAME}:${GIT_COMMIT}" .'
                            sh 'docker push "${DOCKER_IMAGE_NAME}:${GIT_COMMIT}"'
                        }
                    }
                }
            }
        }
    }
}
