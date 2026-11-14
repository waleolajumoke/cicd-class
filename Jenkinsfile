pipeline{
    agent any
    environment{
        DOCKERHUB_USERNAME = "tech365"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/nodejs-cicd-app3"
    }
    stages{
        stage("checkout source code"){
            steps{
                checkout scm
            }
        }

        stage("install dependencies"){
            steps{
                sh "npm ci"
            }
        }

        stage("Build docker image"){
            steps{
                sh """docker build \
                -t ${IMAGE_NAME}:${BUILD_NUMBER}\
                -t ${IMAGE_NAME}:latest .
                """
            }
        }

        stage("push to dockerhub"){
            steps{
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-credentials',
                        usernameVariable: 'DOCKERHUB_USERNAME',
                        passwordVariable: 'DOCKER_TOKEN'
                    )
                ]){
                    sh """echo "$DOCKER_TOKEN" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin
                    docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                    docker push ${IMAGE_NAME}:latest
                    docker logout
                     """
                }
            }
        }
    }
}