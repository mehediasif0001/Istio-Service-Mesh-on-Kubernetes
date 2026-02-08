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



![image alt](https://istio.io/latest/docs/ops/deployment/architecture/arch.svg)




An Istio service mesh is divided into **Data Plane** and **Control Plane**:

### Data Plane 🛠️
- Composed of **Envoy proxies** deployed as **sidecars**.
- **Manages all network traffic** between microservices.
- Collects **telemetry** and reports mesh traffic.
- Key Envoy features:
  - Dynamic service discovery
  - Load balancing
  - TLS termination
  - HTTP/2 & gRPC proxying
  - Circuit breakers & retries
  - Fault injection & staged rollouts
  - Rich metrics collection
- Enables adding Istio features **without code changes**.

### Control Plane 🏗️
- Managed by **Istiod**.
- Responsibilities:
  - Service discovery
  - Configuration management
  - Certificate & identity management
- Converts high-level traffic rules into **Envoy configurations**.
- Provides **security & authentication**:
  - mTLS for secure communication
  - Service-to-service and end-user authentication
  - Role-based access control
- Supports multiple environments: **Kubernetes, VMs, etc.**

### Benefits 
- Fine-grained traffic management
- Network resiliency & fault tolerance
- Strong security policies
- Observability & telemetry

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

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/inject.png)


## ✦ Bookinfo Sample Application Deployment

We will deploy the Bookinfo application to test Istio features like sidecar injection, routing, and observability.

✦ Apply the Bookinfo sample manifest

```bash
kubectl apply -f istio-sample.yml
```

**istioctl analyze**

```bash
istioctl analyze
```
![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/apply-anlyze.png)


**Check Pods**
```bash
kubectl get pods
```
![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/get%20po.png)

**✦ Sidecar Verification**

Randomly describe one pod to check Istio sidecar:

```bash
kubectl describe pod details-v1-67894999b5-cq46t
```
You should see the Envoy proxy container injected by Istio.

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/des%20command.png)


![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/sidecar.png)



📊 Business Impact

✦ Simplifies service-to-service communication in Kubernetes

✦ Provides observability without modifying app code

✦ Improves security with mTLS and policies

✦ Enables advanced traffic management for canary releases and blue/green deployments




👤 Author

### Mehedi Hasan Asif

📬 Connect on LinkedIn: https://www.linkedin.com/in/asifsec/

