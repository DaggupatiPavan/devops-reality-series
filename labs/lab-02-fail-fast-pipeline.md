# Lab 02 – Fail-Fast CI/CD Pipeline (Day 2)

## Objective
Learn how to design a **fail-fast CI/CD pipeline** that provides rapid feedback,
saves compute cost, and improves developer experience.

This lab focuses on **CI/CD as a system**, not just a pipeline script.

---

## Scenario
Your current CI/CD pipeline:
- Takes 15+ minutes to fail
- Runs expensive steps before validation
- Wastes developer time and compute resources

Your goal:
- Make the pipeline fail within the **first 2–3 minutes**
- Detect issues as early as possible

---

## Prerequisites
- GitHub repository
- GitHub Actions enabled
- Basic CI/CD knowledge
- Java/Maven project (or similar build tool)

---

## Step 1: Understand the Anti-Pattern

Typical inefficient pipeline order:

```text
Build → Docker Build → Push Image → Deploy → Unit Tests → Security Scan
````

Problems:

* Failures are detected too late
* Infrastructure is touched unnecessarily
* Developers wait too long for feedback

---

## Step 2: Design a Fail-Fast Pipeline

Improved order:

```text
Lint → Unit Tests → Security Scan → Build → Package → Deploy
```

Principle:

> Validate first, spend resources later.

---

## Step 3: Create a Fail-Fast GitHub Actions Workflow

Create `.github/workflows/ci.yml`:

```yaml
name: Fail-Fast CI

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Unit Tests
        run: mvn test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Build Artifact
        run: mvn package
```

---

## Step 4: Introduce an Intentional Failure

Modify your test phase:

```bash
exit 1
```

Or break a test case intentionally.

Observe:

* Which job fails?
* How long did it take?
* Was the build job skipped?

---

## Step 5: Add a Security Gate (Optional)

Add an early container or dependency scan:

```bash
trivy fs .
```

Fail the pipeline on high/critical issues:

```bash
trivy fs --severity HIGH,CRITICAL --exit-code 1 .
```

---

## Expected Outcome

* Pipeline fails early
* Build and deploy steps are skipped
* Faster feedback to developers
* Reduced CI cost

---

## Reflection Questions

* Where do failures happen most often?
* Should this logic be reused across repositories?
* Which steps are mandatory vs optional?

---

## Optional Extension

Enhancements you can try:

* Convert test logic into a reusable composite action
* Add caching to speed up execution
* Tag failure reasons for observability

---

## Key Learning

> CI/CD speed is defined by how fast you fail, not how fast you deploy.
