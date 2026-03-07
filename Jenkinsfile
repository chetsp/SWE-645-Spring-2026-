pipeline { 

    agent any 

    environment { 

        DOCKERHUB_PASS = credentials('docker-hub-pass') 

        DOCKER_USER = 'chetsp2613' 

        IMAGE_NAME = 'campusconnect' 

        K8S_NAMESPACE = 'campusconnect-ns' 

        DEPLOY_NAME = 'campusconnect-deployment' 

    } 

    stages { 

        stage('Checkout') { 

            steps { checkout scm } 

        } 

        stage('Build & Push Docker Image') { 

            steps { 

                script { 

                    sh 'docker login -u ${DOCKER_USER} -p ${DOCKERHUB_PASS}' 

                    def img = docker.build("${DOCKER_USER}/${IMAGE_NAME}:${BUILD_TIMESTAMP}") 

                    img.push() 

                    img.push('latest') 

                } 

            } 

        } 

        stage('Deploy to Kubernetes') { 

            steps { 

                script { 

                    sh '''kubectl set image deployment/${DEPLOY_NAME} \ 

                      campusconnect=${DOCKER_USER}/${IMAGE_NAME}:${BUILD_TIMESTAMP} \ 

                      -n ${K8S_NAMESPACE}''' 

                } 

            } 

        } 

    } 

} 

  
