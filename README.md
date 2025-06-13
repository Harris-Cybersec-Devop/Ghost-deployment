# 🚀 Ghost CMS Deployment via CI/CD (GitHub Actions + Docker + Kubernetes)

This project demonstrates a complete CI/CD pipeline for deploying the [Ghost CMS](https://ghost.org/) using:

- **CI**: GitHub Actions to build and push Docker images
- **CD**: Ansible for provisioning, Minikube for Kubernetes cluster, and `kubectl` for application deployment

---

## 📦 Project Structure

.
├── .github/workflows/
│ └── docker-image.yml # GitHub Actions CI workflow
├── ansible/
│ └── deploy.yml # Ansible playbook to provision and deploy Ghost on Kubernetes
├── k8s/
│ ├── ghost-deployment.yml # Kubernetes Deployment & Service definitions
├── Dockerfile # Docker image for Ghost CMS
├── README.md # You're here!
└── ...


---

## 🛠️ Tools & Technologies

| Layer | Tool |
|------|------|
| Source Control | Git + GitHub |
| Continuous Integration | GitHub Actions |
| Containerization | Docker |
| Image Registry | Docker Hub |
| Configuration Management | Ansible |
| Container Orchestration | Kubernetes (via Minikube) |

---

## ✅ CI Pipeline (GitHub Actions)

### Location: `.github/workflows/docker-image.yml`

**Steps:**
1. Trigger on `push` to `main`
2. Set up Docker environment
3. Build Docker image from custom Ghost `Dockerfile`
4. Push image to Docker Hub  
   ➤ Image name: `harriscybersecdevop/ghost-custom:latest`

---

## 🚀 CD Pipeline (Ansible + Kubernetes)

### Location: `ansible/deploy.yml`

**Steps:**
1. Start Minikube
2. Pull latest image from Docker Hub
3. Apply Kubernetes deployment and service manifests
4. Expose Ghost CMS at port 80 via NodePort

---

## 📂 Kubernetes Configuration

### File: `k8s/ghost-deployment.yml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ghost-service
spec:
  selector:
    app: ghost
  ports:
    - protocol: TCP
      port: 80
      targetPort: 2368
  type: NodePort
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ghost-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ghost
  template:
    metadata:
      labels:
        app: ghost
    spec:
      containers:
        - name: ghost
          image: harriscybersecdevop/ghost-custom:latest
          ports:
            - containerPort: 2368

⚙️ How to Run Locally
🧪 Prerequisites
Docker

Minikube

Ansible

kubectl

▶️ 1. Trigger CI (push to main)

git push origin main
This automatically:

Builds and pushes Docker image via GitHub Actions

▶️ 2. Trigger CD via Ansible
ansible-playbook ansible/deploy.yml

This automatically:

Starts Minikube

Deploys Ghost CMS to Kubernetes


▶️ 3. Access Ghost
bash
Copy
Edit
minikube service ghost-service --url
Open the printed URL in your browser to view your Ghost CMS instance.


Exposes it on a NodePort<img width="1440" alt="Screenshot 2025-06-13 at 6 26 09 AM" src="https://github.com/user-attachments/assets/bd77b933-9a41-46d1-9628-5b6fc19503aa" />
<img width="1440" alt="Screenshot 2025-06-13 at 6 26 09 AM" src="https://github.com/user-attachments/assets/7540293d-64b1-43aa-bdd0-8d2b7c8baea8" />

