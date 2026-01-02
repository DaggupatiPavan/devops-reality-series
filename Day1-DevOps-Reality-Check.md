# Day 1 – DevOps Reality Check (Hands-on + Concepts)
_Audience: DevOps / Cloud / Platform Engineers (4–8 yrs)_

---

## Goal of Day 1

Understand **what modern DevOps really means in practice** and how your daily work should evolve:
- Less manual work
- More system design
- More ownership

This day combines **concepts with hands-on examples**.

---

## 1. Automation Over Execution

### ❌ Traditional DevOps (Execution Heavy)
- SSH into servers
- Restart services manually
- Manually scale infrastructure
- Fix infra during incidents

### ✅ Modern DevOps (Automation Heavy)
- Auto-detection
- Auto-remediation
- Humans handle **exceptions**, not routine work

---

### 🔧 Hands-on Example: Self-Healing VM (AWS)

**Problem:**  
Application crashes → engineer restarts the service manually.

**Modern Solution:**  
Let the platform recover automatically.

#### Step 1: Health Check Script
```bash
#!/bin/bash
systemctl is-active myapp || exit 1
````

#### Step 2: Auto Scaling Group Health Check

```
HealthCheckType: EC2
HealthCheckGracePeriod: 300
```

If the health check fails:

* Instance is marked unhealthy
* Auto Scaling Group replaces it automatically

📌 **Lesson:**
If a human can fix it with a command, a system can automate it.

---

## 2. Pipelines Are Products (Not YAML Files)

### ❌ Anti-Patterns

* One pipeline per repository
* Copy-paste Jenkinsfiles
* No versioning
* No ownership

### ✅ Product Mindset

* Pipelines are **shared platform components**
* Versioned and documented
* Reusable across teams
* Observable and governed

---

### 🔧 Hands-on: Reusable CI Template (GitHub Actions)

**Shared workflow (`.github/workflows/build.yml`)**

```yaml
on:
  workflow_call:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: mvn clean package
```

**Used in application repository**

```yaml
jobs:
  call-build:
    uses: org/ci-templates/.github/workflows/build.yml@v1
```

📌 **Lesson:**
If multiple repositories share logic, centralize it.

---

## 3. Cloud Cost = Engineering Responsibility

### ❌ Commonly Ignored Areas

* Over-sized instances
* No auto-scaling
* Missing resource tags
* No cost visibility

---

### 🔧 Hands-on: EC2 Right-Sizing

Check CPU utilization:

```bash
aws cloudwatch get-metric-statistics \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistics Average \
  --period 3600
```

Consistently low utilization (<20%) usually means over-provisioning.

---

### 🔧 Hands-on: Kubernetes Cost Leak Example

**Inefficient Pod Spec**

```yaml
resources:
  requests:
    cpu: "2"
    memory: "4Gi"
```

**Optimized Pod Spec**

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
Cost optimization begins in YAML, not spreadsheets.

---

## 4. Observability Over Monitoring

### ❌ Monitoring Only

* CPU high
* Pod restarted
* Alert triggered

### ✅ Observability

* What changed?
* Why did it fail?
* What was impacted?

---

### 🔧 Hands-on: Correlating Logs and Metrics

**Example Log**

```json
{
  "service": "payment-api",
  "latency_ms": 1200,
  "trace_id": "abc123"
}
```

**Related Metric**

```
http_request_latency{service="payment-api"} > 1000ms
```

📌 **Lesson:**
Metrics show *where*, logs explain *why*, traces reveal *how*.

---

## 5. Platform Thinking vs Tool Obsession

### ❌ Tool-Centric Thinking

* Jenkins vs GitHub Actions
* Terraform vs Pulumi

### ✅ Platform-Centric Thinking

* How fast can developers deploy?
* How safe are rollbacks?
* How much manual work remains?

---

### 🔧 Hands-on: Platform Design Checklist

Before selecting tools, ask:

```
- Can developers self-serve?
- Is rollback automated?
- Are failures visible and actionable?
- Is access controlled via policy?
```

📌 **Lesson:**
Tools are implementation details. Platforms define developer experience.

---

## Practical Exercise

1. Choose one CI/CD pipeline you own
2. Identify:

   * Manual steps
   * Repeated logic
   * Late-stage failures
3. Automate or standardize **one improvement**
4. Document the change

---

## Key Takeaways

* Automation beats heroics
* Pipelines need ownership
* Cost is part of engineering design
* Observability reduces on-call fatigue
* Platforms scale teams, not tools

---

## What’s Next – Day 2 Preview

👉 **Day 2 – CI/CD Is Not a Pipeline, It’s a System**

* Fail-fast pipeline design
* Security inside CI/CD
* Jenkins → GitHub Actions migration patterns
* AI-assisted failure analysis

---

⭐ Star the repository if this helped and follow the series.

```
```
Next, I strongly recommend:
➡️ **Day 2 – CI/CD as a System (hands-on)**  
➡️ Add a `/labs` folder for exercises  
```
