
# ☁️ **KUBERNETES MONGODB + MONGO EXPRESS DEMO**

This is a **Kubernetes-based full-stack demo** featuring a secure, containerized MongoDB backend and a web-based admin UI powered by Mongo Express. The stack is deployed using core Kubernetes resources including **Secrets, ConfigMaps, Deployments, Services, and Ingress**—making it ideal for cloud-native platforms and showcasing production-ready practices.

---

## 🚀 **TECH STACK**

- **MongoDB** (NoSQL database)
- **Mongo Express** (Web-based MongoDB admin UI)
- **Kubernetes** (Orchestration)
- **Secrets & ConfigMaps** for secure and modular configuration
- **Ingress** for external access via hostname
- **LoadBalancer** service for browser-based testing (via NodePort fallback)

---

## 📁 **ARCHITECTURE OVERVIEW**

```text
[Browser]
   |
   |--> [Ingress] --> [Mongo Express Service]
                           |
                      [Mongo Express Pod]
                           |
            [Secure ENV via Secret + ConfigMap]
                           |
                    [MongoDB Service (ClusterIP)]
                           |
                      [MongoDB Pod]
```

---

## 🔐 **SECURE CONFIGURATION**

All sensitive data (MongoDB credentials) are stored in a Kubernetes **Secret**, and injected securely into both the MongoDB and Mongo Express pods.

```yaml
# mongo-secret.yaml
type: Opaque
data:
  mongo-root-username: dXNlcm5hbWU=  # username (base64)
  mongo-root-password: cGFzc3dvcmQ=  # password (base64)
```

The MongoDB service name (`mongo`) is abstracted via a **ConfigMap**, making this deployment portable and more maintainable.

---

## 🧱 **DEPLOYMENTS**

- **MongoDB Deployment**
  - Exposes port `27017` internally via ClusterIP service
  - Credentials managed via Secret

- **Mongo Express Deployment**
  - Exposes web interface on port `8081`
  - Depends on ConfigMap for database host and Secrets for credentials

---

## 🌐 **SERVICES**

- `mongo` (ClusterIP): Internal DB access only
- `mongo-express-service` (LoadBalancer): Exposes the UI on `NodePort 30000` for testing

---

## 🌍 **INGRESS**

Provides hostname-based routing to the Kubernetes Dashboard (`dashboard.com`), demonstrating Ingress rules and annotations.

```yaml
rules:
  - host: dashboard.com
    http:
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: kubernetes-dashboard
              port:
                number: 80
```

---

## ✅ **HOW TO DEPLOY**

> Prerequisite: Kubernetes cluster + Ingress controller (like nginx)

```bash
# Apply Secrets & ConfigMap
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongodb-configmap.yaml

# Deploy MongoDB and Service
kubectl apply -f mongodb-deployment.yaml
kubectl apply -f mongodb-service.yaml

# Deploy Mongo Express and Service
kubectl apply -f mongo-express-deployment.yaml
kubectl apply -f mongo-express-service.yaml

# Optional: Ingress
kubectl apply -f dashboard-ingress.yaml
```

---

## 🔍 **ACCESS THE UI**

- **Mongo Express**: [http://localhost:30000](http://localhost:30000) (or your minikube IP)
- **Credentials**: Set in `mongo-secret` (username: `username`, password: `password`)
- **Kubernetes Dashboard**: [http://dashboard.com](http://dashboard.com) (if Ingress configured and /etc/hosts updated)

---

## 📌 **LEARNING GOALS DEMONSTRATED**

- ✔️ Kubernetes multi-tier deployments
- ✔️ Secure configuration via Secrets & ConfigMaps
- ✔️ Load-balanced & internal services
- ✔️ Host-based routing with Ingress
- ✔️ Environment-variable driven configuration

---

## 📚 **IDEAL FOR**

This project is ideal for:

- 📘 Students or professionals preparing for the **CKA/CKAD**
- 💼 DevOps engineers building proof-of-concepts
- 🧪 Recruiters looking for candidates with **real Kubernetes experience**

---

## 🤩 **NEXT STEPS (IMPROVEMENTS)**

- Add **Helm chart** packaging
- Enable **Argo CD** for GitOps-based deployment
- Integrate **Prometheus/Grafana** for observability
- Add CI/CD with GitHub Actions

---

## 🧠 **AUTHOR**

Built with ❤️ by **Narcisse Obadiah** — exploring production-grade cloud-native patterns on Kubernetes.

GitHub: [NarcisseObadiah](https://github.com/NarcisseObadiah)