pipeline {
    agent any

    environment {
        IMAGE_NAME = 'myapp'
        IMAGE_TAG = "${env.BUILD_ID}" // Using build ID for unique tags
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
                    // Check if Dockerfile exists
                    def dockerfileExists = fileExists 'Dockerfile'
                    if (!dockerfileExists) {
                        error "Dockerfile not found in the repository"
                    }
                    
                    // Build Docker image
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login and push image (optional - remove if you don't want to push)
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    // Run Ansible playbook (make sure playbook exists and SSH credentials are set up)
                    ansiblePlaybook(
                        playbook: 'ansible/deploy.yaml',
                        credentialsId: 'ssh-credentials',
                        extras: "-e image_tag=${IMAGE_TAG}"
                    )
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for build ${env.BUILD_ID}"
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the logs above for details."
        }
    }
}