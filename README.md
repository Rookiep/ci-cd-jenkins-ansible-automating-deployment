# CI/CD Pipeline with Jenkins, Docker, Ansible, and Minikube

## Steps to run

1. Start Minikube:

```
minikube start
```

2. Build and test Docker image locally:

```
docker build -t myapp:latest .
docker run -p 5000:5000 myapp:latest
```

3. Push Docker image to Docker Hub:

```
docker tag myapp:latest <dockerhub-username>/myapp:latest
docker push <dockerhub-username>/myapp:latest
```

4. Run Jenkins pipeline:
- Builds Docker image
- Pushes to Docker Hub
- Deploys to Minikube via Ansible
