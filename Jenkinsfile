pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = 'latest'
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
                    // Build Docker image and store reference
                    env.IMAGE = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login and push image
                    withDockerRegistry([credentialsId: 'dockerhub-credentials', url: '']) {
                        env.IMAGE.push("${IMAGE_TAG}")
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    // Run Ansible playbook
                    ansiblePlaybook credentialsId: 'ssh-credentials', playbook: 'ansible/deploy.yaml'
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