pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('rihem_docker')
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
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    sh """
                    docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
                    aquasec/trivy:latest image --exit-code 0 --severity LOW,MEDIUM,HIGH,CRITICAL --timeout 5m \
                    ${IMAGE_NAME_SERVER}
                    """
                }
            }
    
            }
        }
    }


    post {
        always {
            echo 'Cleaning up...'
        }
    }
}
