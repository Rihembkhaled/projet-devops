pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Rihembkhaled/projet-devops'
            }
        }
        stage('Pull Image') {
            steps {
                dir('server') {
                    script {
                        withDockerRegistry([credentialsId: DOCKERHUB_CREDENTIALS, url: 'https://index.docker.io/v1/']) {
                            dockerImage = docker.pull("rihemb/devops")
                        }
                    }
                }
            }
        }
        stage('Scan Server Image') {
            steps {
                script {
                    sh """
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
                    aquasec/trivy:latest image --exit-code 0 --severity LOW,MEDIUM,HIGH,CRITICAL \
                    rihemb/devops
                    """
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
