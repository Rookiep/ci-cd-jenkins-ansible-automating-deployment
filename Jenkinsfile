pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = "${env.BUILD_ID}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Rookiep/ci-cd-jenkins-ansible-automating-deployment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Simple docker build using shell
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    // Simple ansible using shell
                    sh "ansible-playbook -i ansible/inventory ansible/deploy.yaml --extra-vars 'image_tag=${IMAGE_TAG}'"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}