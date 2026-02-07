#  Istio Service Mesh on Kubernetes

This repository is a **complete learning guide and hands-on implementation of Istio Service Mesh on Kubernetes**.  
It is designed for **learners, DevOps engineers, and cloud practitioners** who want to understand Istio from **basics to advanced concepts**.

This is **not only an installation repo** — it will progressively cover **all major Istio topics** with real examples.

---

## 🧩 **Problem Statement**

In Kubernetes-based microservices architectures, managing:
- Traffic routing
- Service-to-service security
- Observability
- Resilience and fault handling

becomes complex as applications scale.

This repository demonstrates how **Istio Service Mesh** solves these problems **without modifying application code**.

---

## 🔹 **Architecture Overview**

Client  
→ Istio Ingress Gateway  
→ Envoy Sidecar Proxy  
→ Kubernetes Services & Pods  

Istio Control Plane (`istiod`) manages:
- Traffic policies
- Security (mTLS)
- Telemetry and observability

---

## ⚙️ **Implementation Details**

| Component | Value |
|---------|------|
| Platform | Kubernetes |
| Service Mesh | Istio |
| Installation Tool | istioctl |
| Profile Used | demo |
| Control Plane | istiod |
| Data Plane | Envoy Proxy |
| Namespace Injection | Automatic |
| Observability | Prometheus, Grafana, Kiali |

---

## 📥 **task 01: Download Istio**

### 🔹 Download Istio

```bash
curl -L https://istio.io/downloadIstio | sh -
```
![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/Screenshot_14.png)

Move to Istio Directory

```bash
cd istio-1.28.3
```
**Add istioctl to PATH**
```bash
export PATH=$PWD/bin:$PATH
```

🔍 Explanation

🔹Adds Istio binaries to system PATH

🔹Allows running istioctl globally



![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/Screenshot_1.png)



# Install Istio Service Mesh (Demo Profile)

```bash
istioctl install --set profile=demo -y
```

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/Screenshot_2.png)

# varify 
```bash
kubectl get all -n istio-system
kubectl get pods -n istio-system
```
![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/Screenshot_3.png)

 # istioctl version  
 ```bash
istioctl version
```

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/Screenshot_4.png)

# Enable Automatic Sidecar Injection in Default Namespace

```bash
kubectl label namespace default istio-injection=enabled
```



📊 Business Impact

✦ Simplifies service-to-service communication in Kubernetes
✦ Provides observability without modifying app code
✦ Improves security with mTLS and policies
✦ Enables advanced traffic management for canary releases and blue/green deployments

👤 Author

### Mehedi Hasan Asif

📬 Connect on LinkedIn: https://www.linkedin.com/in/asifsec/

