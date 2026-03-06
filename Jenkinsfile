pipeline {
    agent any

    environment {
        DOCKERHUB_PASS = credentials('docker-hub-pass')
        DOCKER_USER = 'chetsp2613'
        IMAGE_NAME = 'campusconnect'
        K8S_NAMESPACE = 'campusconnect-ns'
        DEPLOY_NAME = 'campusconnect-deployment'
        TIMESTAMP = sh(script: "date +%Y%m%d%H%M%S", returnStdout: true).trim()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    sh "docker login -u ${DOCKER_USER} -p ${DOCKERHUB_PASS}"
                    sh "docker build -t ${DOCKER_USER}/${IMAGE_NAME}:${TIMESTAMP} ."
                    sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:${TIMESTAMP}"
                    sh "docker tag ${DOCKER_USER}/${IMAGE_NAME}:${TIMESTAMP} ${DOCKER_USER}/${IMAGE_NAME}:latest"
                    sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Added KUBECONFIG export and full path to kubectl
                    sh """
                        export KUBECONFIG=/etc/rancher/rke2/rke2.yaml
                        /var/lib/rancher/rke2/bin/kubectl set image deployment/${DEPLOY_NAME} \
                          campusconnect=${DOCKER_USER}/${IMAGE_NAME}:${TIMESTAMP} \
                          -n ${K8S_NAMESPACE}
                    """
                }
            }
        }
    }
        
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Pipeline failed. Check the logs above.'
        }
    }
}
