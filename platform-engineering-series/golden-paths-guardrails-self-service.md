# Golden Paths, Guardrails & Self-Service

## Overview

High-performing platform teams do not control developers.

They **enable them**.

Golden paths, guardrails, and self-service are the core mechanisms that allow teams to move fast **without sacrificing safety, security, or reliability**.

This document explains how these concepts work together to form a scalable platform.

---

## Golden Paths

### What Are Golden Paths?

Golden paths are the **recommended, optimized routes to production**.

They represent the best-known way to:

* Build
* Test
* Deploy
* Operate

Applications in a given environment.

Golden paths are optional — but they should be so good that teams **naturally choose them**.

---

### What Golden Paths Usually Include

* Standard CI/CD pipeline templates
* Approved base container images
* Reusable Terraform or infrastructure modules
* Standard Helm charts or deployment patterns

Golden paths embed:

* Security
* Reliability
* Observability

By default.

---

### Why Golden Paths Matter

Without golden paths:

* Every team invents its own solution
* Best practices are inconsistently applied
* Platform teams become reviewers and gatekeepers

With golden paths:

* Teams move faster
* Operational consistency improves
* Cognitive load is reduced

---

## Guardrails (Not Gates)

### What Guardrails Are

Guardrails are **automated constraints** that keep systems safe without blocking innovation.

They enforce standards while still allowing flexibility.

Guardrails are not manual approvals or hard stops.

---

### Common Guardrails in Platforms

* Policy-as-code
* Resource quotas and limits
* Secure defaults for networking and identity
* Automated security scanning

Guardrails say:

> Yes, but safely.

---

### Guardrails vs Gates

| Guardrails       | Gates           |
| ---------------- | --------------- |
| Automated        | Manual          |
| Enable speed     | Slow teams      |
| Default behavior | Exception-based |
| Scale well       | Do not scale    |

Platforms should prefer guardrails wherever possible.

---

## Self-Service

### What Self-Service Means

Self-service means developers can:

* Provision infrastructure
* Deploy applications
* Access logs, metrics, and traces

Without opening tickets or waiting for platform teams.

---

### Why Self-Service Is Mandatory

Without self-service:

* Platform teams become bottlenecks
* Delivery slows down
* Context switching increases

Self-service allows platform teams to **scale their impact without scaling headcount**.

---

## How These Concepts Work Together

Golden paths provide the **fast path**.

Guardrails provide the **safety net**.

Self-service provides the **delivery mechanism**.

Remove any one of these, and the platform breaks:

* No golden paths → no adoption
* No guardrails → no safety
* No self-service → no scale

---

## Platform Design Principles

### Optimize for the Common Case

Design golden paths for the majority of workloads.

Edge cases can be handled separately.

---

### Make the Right Thing the Easy Thing

If following best practices requires extra effort, adoption will fail.

Platforms must reduce friction, not add it.

---

### Measure Adoption, Not Features

Successful platforms measure:

* Usage of golden paths
* Deployment frequency
* Developer satisfaction

Not the number of features shipped.

---

## Key Takeaways

* Golden paths drive adoption
* Guardrails ensure safety
* Self-service enables scale
* Platforms succeed by reducing cognitive load

---

## What’s Next

**Day 5 – Real-World Platform Engineering Mistakes Teams Make**

* Why platforms fail
* Common anti-patterns
* How to avoid them

---

> This document is part of the *Platform Engineering Series*.
> Designed for engineers building scalable internal platforms.
