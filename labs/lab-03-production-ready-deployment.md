# Lab 03 – Production-Ready Kubernetes Deployment (Day 3)

## Objective
Transform a basic Kubernetes deployment into a **production-grade, reliable, and cost-aware workload**.

This lab focuses on **how real Kubernetes systems fail and recover**.

---

## Scenario
A simple Kubernetes Deployment works in dev but fails in production due to:
- No health checks
- No resource controls
- No autoscaling
- Poor reliability defaults

Your goal is to fix these issues step by step.

---

## Prerequisites
- Kubernetes cluster (Kind / Minikube / EKS / AKS / GKE)
- `kubectl` configured
- Basic Kubernetes knowledge

---

## Step 1: Deploy a Basic Application (Unsafe)

Create `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
      - name: app
        image: nginx
````

Apply and verify:

```bash
kubectl apply -f deployment.yaml
kubectl get pods
```

---

## Step 2: Add Health Probes

Update the container spec:

```yaml
livenessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 10
  periodSeconds: 5
```

Apply and observe:

```bash
kubectl apply -f deployment.yaml
kubectl describe pod <pod-name>
```

---

## Step 3: Add Resource Requests and Limits

Add safety controls:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"
  limits:
    cpu: "300m"
    memory: "256Mi"
```

Apply and observe scheduling behavior.

---

## Step 4: Add Horizontal Pod Autoscaler

Create `hpa.yaml`:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: demo-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: demo-app
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60
```

Apply:

```bash
kubectl apply -f hpa.yaml
kubectl get hpa
```

---

## Step 5: Observe Resource Usage

```bash
kubectl top pods
kubectl top nodes
```

Check:

* Actual usage vs requested
* Over-provisioning risks

---

## Expected Outcome

* Pods restart automatically on failure
* Traffic flows only to healthy pods
* Resource usage is controlled
* Application scales under load

---

## Reflection Questions

* What happens if probes are removed?
* How do limits protect the cluster?
* What cost issues can arise without HPA?

---

## Optional Extensions

Try these:

* Break the app endpoint and observe probe behavior
* Increase load using `kubectl run busybox`
* Reduce node size and observe scheduling

---

## Key Learning

> Kubernetes reliability and cost efficiency are defined by YAML quality, not cluster size.
