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
                    sh """
                        cat <<EOF > kubeconfig.yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJlakNDQVIrZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWtNU0l3SUFZRFZRUUREQmx5YTJVeUxYTmwKY25abGNpMWpZVUF4TnpjeU1UUXpOVEk1TUI0WERUSTJNREl5TmpJeU1EVXlPVm9YRFRNMk1ESXlOREl5TURVeQpPVm93SkRFaU1DQUdBMVVFQXd3WmNtdGxNaTF6WlhKMlpYSXRZMkZBTVRjM01qRTBNelV5T1RCWk1CTUdCeXFHClNNNDlBZ0VHQ0NxR1NNNDlBd0VIQTBJQUJEM1o0cVR1eFdUYjlibWo1RHZaTnVKcWZwcWgwV3FqYmdUeUs5bGwKSHVTc0NuS1RwRGRnc2lwdWhoQjVPb0c5RXhaSGpreTVIOC9xT2hKbWcydVExbW1qUWpCQU1BNEdBMVVkRHdFQgovd1FFQXdJQ3BEQVBCZ05WSFJNQkFmOEVCVEFEQVFIL01CMEdBMVVkRGdRV0JCUlRGNWxxNm9ZZ2lLOE9FYkFOCkFRTGhsZGIxS0RBS0JnZ3Foa2pPUFFRREFnTkpBREJHQWlFQTVseTIxaHVjZ2RVaEMrN3BiUy9iRUNseUNRYlQKbTVrRkx6Y0hqK3p0MTJvQ0lRRFZTOTFnVWoraEVnbFhpTUdDVGlnSGVRekhyYTgzRGRIWktCUjhRdGw3Tmc9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    server: https://107.22.248.10:6443
  name: default
contexts:
- context:
    cluster: default
    user: default
  name: default
current-context: default
kind: Config
users:
- name: default
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJrVENDQVRpZ0F3SUJBZ0lJQ3VrMWxRNm95a2t3Q2dZSUtvWkl6ajBFQXdJd0pERWlNQ0FHQTFVRUF3d1oKY210bE1pMWpiR2xsYm5RdFkyRkFNVGMzTWpFME16VXlPVEFlRncweU5qQXlNall5TWpBMU1qbGFGdzB5TnpBeQpNall5TWpBMU1qbGFNREF4RnpBVkJnTlZCQW9URG5ONWMzUmxiVHB0WVhOMFpYSnpNUlV3RXdZRFZRUURFd3h6CmVYTjBaVzA2WVdSdGFXNHdXVEFUQmdjcWhrak9QUUlCQmdncWhrak9QUU1CQndOQ0FBUlZlYk8rRTducnhRSi8KRzFIeFJONWJmSTFlaXhyUXBabk1wVEVYcWZpRmxFbUtQZWJTT3RKa21CU09PU0VLYjZ6ejMyNDNpdjFVelUxUApxWHlqRjFwZm8wZ3dSakFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUhBd0l3Ckh3WURWUjBqQkJnd0ZvQVU5ZGFUNWpEUmxHb0VjM0xQOCt3WXE5QUVaRm93Q2dZSUtvWkl6ajBFQXdJRFJ3QXcKUkFJZ0FRTWdlWmZkYXg3UmZFRlNFTDloSVpvSUpJYzA0aStvMnNFNmI1d2FnNWdDSUhtRXZZdTYraEM1VFRoVQp3VWM3bmNuZmR0SE11bTFhTG1MbEV5SFlPcWFUCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0KLS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJlVENDQVIrZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWtNU0l3SUFZRFZRUUREQmx5YTJVeUxXTnMKYVdWdWRDMWpZVUF4TnpjeU1UUXpOVEk1TUI0WERUSTJNREl5TmpJeU1EVXlPVm9YRFRNMk1ESXlOREl5TURVeQpPVm93SkRFaU1DQUdBMVVFQXd3WmNtdGxNaTFqYkdsbGJuUXRZMkZBTVRjM01qRTBNelV5T1RCWk1CTUdCeXFHClNNNDlBZ0VHQ0NxR1NNNDlBd0VIQTBJQUJEeFkvcXBqK1VSdzhmeks4WkNPUHZCUlUrYUpPaVBpdzAwd011OXgKNFdGZzBkYUVhaUVpckk1U3kxOHlvOGIreUt2WEdWbXRXSS9jeGhJN0hxY3RVU3VqUWpCQU1BNEdBMVVkRHdFQgovd1FFQXdJQ3BEQVBCZ05WSFJNQkFmOEVCVEFEQVFIL01CMEdBMVVkRGdRV0JCVDExcFBtTU5HVWFnUnpjcy96CjdCaXIwQVJrV2pBS0JnZ3Foa2pPUFFRREFnTklBREJGQWlCc1E0S05YOWZIbHg3Z1JDSGJObVNaUml0SVdSTTYKd0FDY1R4cXcvYXdSbWdJaEFKQ1hZd1gxaURXcHRMUXliMlVHQzlQcG9SYUF0TDhURTFqcmRuTi8xZm1WCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K    client-key-data: LS0tLS1CRUdJTiBFQyBQUklWQVRFIEtFWS0tLS0tCk1IY0NBUUVFSUVCQ2k2UExEbEhjNFd1OTNXeDZveEh3MWFCU1VyWWw3ZW9lV2JTNEIrSW9vQW9HQ0NxR1NNNDkKQXdFSG9VUURRZ0FFVlhtenZoTzU2OFVDZnh0UjhVVGVXM3lOWG9zYTBLV1p6S1V4RjZuNGhaUkppajNtMGpyUwpaSmdVampraENtK3M4OTl1TjRyOVZNMU5UNmw4b3hkYVh3PT0KLS0tLS1FTkQgRUMgUFJJVkFURSBLRVktLS0tLQo=
EOF
                        export KUBECONFIG=./kubeconfig.yaml
                        kubectl set image deployment/${DEPLOY_NAME} \
                          campusconnect=${DOCKER_USER}/${IMAGE_NAME}:${TIMESTAMP} \
                          -n ${K8S_NAMESPACE} \
                          --server=https://107.22.248.10:6443 \
                          --insecure-skip-tls-verify
                        rm kubeconfig.yaml
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
