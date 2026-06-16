# Assignment
# DevOps Internship Assignment

# Part 1 - Git & SSH

## SSH Setup

I generated an SSH key on my EC2 instance and added the public key to my GitHub account.

Commands used:

```bash
ssh-keygen -t ed25519 -C "rohiithh5@gmail.com"
cat ~/.ssh/id_ed25519.pub
ssh -T git@github.com
```
this genrates ssh key and this key can be added into githun settings>ssh and gph keys.

After adding the key, I changed my repository remote URL from HTTPS to SSH.

```bash
git remote set-url origin git@github.com:rohithh55/Assignment.git
```

---

## Difference between git fetch and git pull

### git fetch

Downloads the latest changes from the remote repository but does not merge them into the current branch.

```bash
git fetch origin
```

### git pull

Downloads the latest changes and automatically merges them into the current branch.we can say that git add + git merge gives you git pull

```bash
git pull origin main
```

## Merge Conflict

I created two branches.

* **featureA** - changed `app.txt` to `python-app`
* **featureB** - changed `app.txt` to `java-app`

### Commands

Create branches

```bash
git checkout -b featureA
git checkout -b featureB
```

Merge featureA

```bash
git checkout main
git merge featureA
```

This merged successfully because there were no conflicts.

Then I merged featureB.

```bash
git merge featureB
```

Git showed a merge conflict because both branches modified the same line in `app.txt`.

Git displayed:

```text
<<<<<<< HEAD
python-app
=======
java-app
>>>>>>> featureB
```

I manually edited the file, removed the conflict markers, saved the required content, and completed the merge.

```bash
git add app.txt
git commit -m "Resolved merge conflict"
```

---

# Part 2 - Docker

I created a simple Python Flask application and containerized it using Docker.

## Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python","app.py"]
```

## Commands

Build Docker image

```bash
docker build -t devops .
```

Run Docker container

```bash
docker run -d -p 5000:5000 devops
```

List running containers

```bash
docker ps
```

Stop container

```bash
docker stop <container-id>
```

---

## Dockerfile vs Docker Image vs Docker Container

* Dockerfile-is a text file containing instructions to build an image.
* Docker Image- is the packaged application with all required dependencies.
* Docker Container- is a running instance of the Docker image.

### How to reduce Docker image size

* Use a lightweight base image python:3.11-slim- we can use python;3.11 . here slim gives us light weight image thus the image size is reduced
* Use `pip install --no-cache-dir`
* Remove unnecessary files from the image
---------------------------------------------------------

# Part 3 - Kubernetes

Created Kubernetes YAML files for the application.

Deployment:

* 2 replicas
* Uses Docker image

Service:

* LoadBalancer type

## Pod vs Deployment vs Service

* **Pod**: Smallest unit that runs the application.
* **Deployment**: Manages Pods and handles scaling and updates.
* **Service**: Exposes Pods inside or outside the cluster.

### Why EKS?

EKS is a managed by AWS. It manages the Kubernetes control plane, making it easier to deploy, scale, and maintain applications compared to managing Kubernetes on virtual machines.

-----------------------------------------------------------------------

# Part 4 - Jenkins Pipeline

I used Jenkins for the CI/CD pipeline.

Pipeline stages:

1. Clone source code from GitHub
2. Build Docker image
3. Run simple test
4. Push Docker image 
5. Deploy to Kubernetes

----------------------------------------------------

# Part 5 - Monitoring

## Metrics

Used to monitor CPU, memory, and application performance.

## Logs

Contain application events and error messages.

## Traces

Track a request across multiple services.

### Debugging a crashed Pod

Useful commands:

```bash
kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl logs <pod-name> --previous

kubectl exec -it <pod-name> -- sh
```

Suggested monitoring tools:

* Prometheus
* Grafana
* AWS CloudWatch
* ELK Stack

---

# Part 6 - Microservice Deployment Approach

1. Clone the source code from GitHub.
2. Create a Dockerfile.
3. Build and test the Docker image.
4. Push the image to Docker Hub or Amazon ECR.
5. Create Kubernetes Deployment and Service YAML files.
6. Configure Jenkins to build and deploy on every merge to the `main` branch.
7. Send application logs to CloudWatch.
8. Monitor the application using Prometheus and Grafana.

