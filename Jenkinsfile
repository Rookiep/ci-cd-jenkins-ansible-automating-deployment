pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = "${env.BUILD_ID}"     // Jenkins build number as tag
        ANSIBLE_DIR = "ansible"
        PLAYBOOK = "deploy.yaml"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Rookiep/ci-cd-jenkins-ansible-automating-deployment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🏗 Building Docker Image → ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh """
                        docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Run Ansible Deployment') {
            steps {
                script {
                    echo "🚀 Running Ansible Deployment"
                    sh """
                        cd ${ANSIBLE_DIR}
                        ansible-playbook ${PLAYBOOK} --extra-vars "image_tag=${IMAGE_TAG}"
                    """
                }
            }
        }

    }

    post {
        success {
            echo "🎉 Deployment Completed Successfully!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
        always {
            echo "Pipeline finished."
        }
    }
}