pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_NAME_SERVER = 'rihemb/devops'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Rihembkhaled/projet-devops'
            }
        }
        stage ( 'Build Server Image') {
            steps {
                script {
                    dockerImageServer = docker.build("${IMAGE_NAME_SERVER}")
                }
            }
        }
        stage('Scan Server Image') {
            steps {
                script {
                    sh """
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
                    aquasec/trivy:latest image --exit-code 0 --severity LOW,MEDIUM,HIGH,CRITICAL \
                    ${IMAGE_NAME_SERVER}
                    """
                }
            }
        }
    }

    stage('Push Images to Docker Hub') {
                steps {
                    script {
                        docker.withRegistry('', "${DOCKERHUB_CREDENTIALS}") {
                            dockerImageServer.push()
                        }
                    }
                }
            }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker system prune -af'
        }
    }
}
