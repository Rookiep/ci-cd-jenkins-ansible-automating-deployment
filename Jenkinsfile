pipeline {

    agent any

    environment {
        IMAGE_NAME = "myapp"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Rookiep/ci-cd-ja.git'
            }
        }

        stage('Build Docker Image Locally') {
            steps {
                sh """
                    echo '🔨 Building Docker Image Locally...'
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('Run Ansible Deployment') {
            steps {
                ansiblePlaybook(
                    playbook: 'ansible/deploy.yaml',
                    extras: "--extra-vars \"image_tag=${IMAGE_TAG}\""
                )
            }
        }
    }
}
