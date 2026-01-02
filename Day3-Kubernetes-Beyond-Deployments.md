# Day 3 – Kubernetes Is Not Just a Deployment Platform
_Audience: DevOps / Cloud / Platform Engineers (4–8 yrs)_

---

## Goal of Day 3

Move from **“kubectl executor”** to **“Kubernetes system designer”** by learning:
- How production workloads fail
- How reliability is engineered in YAML
- How cost leaks happen silently
- How clusters should be treated as platforms

This day focuses on **real production Kubernetes behavior**.

---

## 1. YAML Is Easy. Reliability Is Hard.

### ❌ Common Beginner Thinking
- Pod is running
- Service is reachable
- Deployment succeeded

### ✅ Production Thinking
- How does it fail?
- How does it recover?
- How does it scale?
- How much does it cost?

---

## 2. Health Probes Decide Availability

### ❌ Missing or Incorrect Probes
- Kubernetes doesn’t know when the app is broken
- Traffic is sent to unhealthy pods

---

### 🔧 Hands-on: Liveness vs Readiness

**Bad Deployment**
```yaml
containers:
- name: app
  image: myapp:1.0
````

**Production-Ready Deployment**

```yaml
containers:
- name: app
  image: myapp:1.0
  livenessProbe:
    httpGet:
      path: /health
      port: 8080
    initialDelaySeconds: 30
    periodSeconds: 10
  readinessProbe:
    httpGet:
      path: /ready
      port: 8080
    initialDelaySeconds: 10
    periodSeconds: 5
```

📌 **Lesson:**
Kubernetes can’t heal what it can’t detect.

---

## 3. Resource Requests & Limits Prevent Cluster Chaos

### ❌ No Limits

* One pod consumes all node resources
* Other workloads starve

---

### 🔧 Hands-on: Resource Safety

**Unsafe**

```yaml
resources: {}
```

**Safe**

```yaml
resources:
  requests:
    cpu: "200m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

📌 **Lesson:**
Requests protect scheduling. Limits protect the cluster.

---

## 4. Autoscaling Is Not Optional

### ❌ Static Replicas

* Manual scaling
* Slow incident response

---

### 🔧 Hands-on: Horizontal Pod Autoscaler

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60
```

📌 **Lesson:**
Autoscaling turns traffic spikes into non-events.

---

## 5. Kubernetes Cost Leaks Are Silent

### ❌ Common Cost Leaks

* Over-sized node groups
* Missing resource limits
* Idle workloads running 24/7

---

### 🔧 Hands-on: Detect Over-Provisioning

```bash
kubectl top pods
kubectl top nodes
```

If usage is consistently low → reduce requests or node size.

📌 **Lesson:**
Unused resources are paid resources.

---

## 6. Security Must Be Default, Not Optional

### ❌ Default-Everything Clusters

* Wide RBAC
* No network isolation
* Shared namespaces

---

### 🔧 Hands-on: Basic RBAC Example

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
```

Bind minimal access only.

📌 **Lesson:**
Security incidents start with over-permission.

---

## 7. GitOps vs Push-Based Deployments

### ❌ Push-Based

* CI pipeline deploys directly
* No cluster visibility
* Drift happens silently

### ✅ GitOps

* Git is the source of truth
* Continuous reconciliation
* Easy rollback

---

### 🔧 Hands-on: GitOps Flow

```text
Git Commit → GitOps Controller → Cluster
```

Rollback = revert commit.

📌 **Lesson:**
GitOps makes Kubernetes predictable.

---

## Practical Exercise

1. Pick one production Deployment YAML
2. Check:

   * Probes present?
   * Requests & limits defined?
   * Autoscaling enabled?
3. Fix **one reliability gap**
4. Measure stability improvement

---

## Key Takeaways

* Kubernetes is a reliability platform
* YAML encodes engineering decisions
* Cost leaks come from bad defaults
* Security must be enforced early
* GitOps reduces operational risk

---

## What’s Next – Day 4 Preview

👉 **Day 4 – Observability & On-Call Reality**

* Designing alerts that matter
* Reducing alert fatigue
* Faster RCA with logs, metrics, traces
* On-call engineering mindset

---

⭐ Star the repository if this helped and continue the series.
