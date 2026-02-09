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

# Istio Gateway & VirtualService – httpbin Routing

This task demonstrates how to expose a Kubernetes service (httpbin) to external traffic using Istio Gateway and VirtualService, and how to control traffic routing based on URL paths.




Client ----> Istio Ingress Gateway ----> VirtualService (path-based routing) ---> httpbin Service (port 8000)




**Step 1: Gateway Configuration**
I created an Istio Gateway named httpbin-gateway that listens for external HTTP traffic on port 80 for the hostname: "httpbin.example.com"



 **Why this is needed?**

- Kubernetes Services are **internal by default**
- External traffic cannot reach a Service directly
- Istio requires a **Gateway** to allow traffic from outside the cluster

The **Gateway** defines:
- Which **port** is open
- Which **protocol** is allowed (HTTP / HTTPS / TCP)
- Which **hostnames** are accepted

Without a Gateway, **external traffic cannot enter the Istio service mesh**.

---
![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/1.png)

this is default labels on istio-ingressgateway ( this pod is avaliable under the ```istio-system``` namespace) . I will use it labels for gateway selector

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/3.png)

Gateway Configuration: https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/gateway.yml

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/2.png)



**How it works?**

- The `selector` attaches this Gateway to **Istio’s default ingress gateway pod**
- **Port 80** is opened to accept **HTTP** requests
- Only requests with the `Host` header set to:

## step2:
**VirtualService Configuration**
https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/virtual-service.yml

 configured **path-based routing** with the following rules:

### Allowed Paths
- `/delay`
- `/status`

### Traffic Destination
- **Service:** `httpbin`
- **Port:** `8000`

### How routing works
- Incoming traffic first enters through the **Istio Gateway**
- The **VirtualService** checks the request path
- If the path matches `/delay` or `/status`:
  - The request is forwarded to the `httpbin` service
  - Traffic is sent to port `8000`
- Requests with other paths are **not matched** by this VirtualService

This configuration enables **fine-grained HTTP routing** inside the Istio service mesh.

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/get%20vs.png)


find accessable host name and port;

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/11.png)

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/2.png)

describe services; 

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/desSVc.png)
![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/des2.png)

try to access:

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/validate.png)

---------
# Istio mTLS & AuthorizationPolicy Demo

This demo showcases **Istio PeerAuthentication and AuthorizationPolicy** to enforce mutual TLS (mTLS) and restrict access between namespaces.

---

## 1️⃣ Configured PeerAuthentication

- Created `demo-peer-policy` in the `demo-app` namespace
- Enforced **STRICT mutual TLS** for all workloads
- Ensured **all service-to-service traffic** inside `demo-app` uses mTLS only
yml file: https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/PeerAuthentication.yml
---
![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/pa.png)

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/pa%20get.png)


## 2️⃣ Configured AuthorizationPolicy

- Created `demo-api-policy` in the `demo-api` namespace
- **Allowed GET requests** only from the `demo-app` namespace
- **Blocked GET requests** from all other namespaces (e.g., `default`)
yml file : https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/AuthorizationPolicy.yml
---

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/ap%20crate.png)

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/ap-apply.png)

### Identified services

```bash
# Check nginx service ClusterIP in demo-api
kubectl get svc -n demo-api

# Pods in demo-app namespace
kubectl get po -n demo-app

# Pods in default namespace
kubectl get po -n default

# From default namespace pod → access denied
kubectl exec <default-pod> -c curl -- curl -s <nginx-service-IP>

# From demo-app namespace pod → access allowed
kubectl exec <demo-app-pod> -n demo-app -c curl -- curl -s <nginx-service-IP>
```
| Pod Namespace      | Command                                                                 | Result   | Explanation                                        |
|-------------------|-------------------------------------------------------------------------|----------|---------------------------------------------------|
| default           | `kubectl exec <default-pod> -c curl -- curl -s <nginx-service-IP>`      | ❌ Blocked | Access denied by Istio AuthorizationPolicy       |
| demo-app          | `kubectl exec <demo-app-pod> -n demo-app -c curl -- curl -s <nginx-service-IP>` | ✅ Allowed | Access allowed because demo-app namespace is permitted |

![image alt](https://github.com/mehediasif0001/Istio-Service-Mesh-on-Kubernetes/blob/main/image_istio/curldemo.png)

4️⃣ Outcome / Verification

PeerAuthentication successfully enforced mTLS

AuthorizationPolicy correctly restricted GET access

Demonstrated Istio RBAC in action:

✅ Allowed traffic from demo-app namespace

❌ Blocked traffic from other namespaces (default)





....

📊 Business Impact

✦ Simplifies service-to-service communication in Kubernetes

✦ Provides observability without modifying app code

✦ Improves security with mTLS and policies

✦ Enables advanced traffic management for canary releases and blue/green deployments




👤 Author

### Mehedi Hasan Asif

📬 Connect on LinkedIn: https://www.linkedin.com/in/asifsec/

