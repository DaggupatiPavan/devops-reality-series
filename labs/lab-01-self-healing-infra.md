# Lab 01 – Self-Healing Infrastructure (Day 1)

## Objective
Understand how **automation replaces manual intervention** by implementing a self-healing service.

This lab demonstrates how systems can **recover automatically without human action**.

---

## Scenario
An application process crashes unexpectedly.
Your goal is to ensure:
- No manual restart
- No SSH-based recovery
- Automatic self-healing

---

## Prerequisites
- Linux VM or EC2 instance
- Root or sudo access
- Basic shell scripting knowledge
- systemd available

---

## Step 1: Create a Dummy Application

Create a simple long-running process.

```bash
sudo nano /usr/local/bin/myapp.sh
````

```bash
#!/bin/bash
while true; do
  echo "MyApp is running..."
  sleep 5
done
```

Make the script executable:

```bash
sudo chmod +x /usr/local/bin/myapp.sh
```

---

## Step 2: Create a systemd Service

Define a systemd unit to manage and restart the app automatically.

```bash
sudo nano /etc/systemd/system/myapp.service
```

```ini
[Unit]
Description=MyApp Self-Healing Service
After=network.target

[Service]
ExecStart=/usr/local/bin/myapp.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Reload systemd and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl start myapp
sudo systemctl enable myapp
```

Verify status:

```bash
systemctl status myapp
```

---

## Step 3: Simulate Failure

Kill the application process:

```bash
pkill -f myapp.sh
```

Observe recovery:

```bash
systemctl status myapp
journalctl -u myapp -f
```

---

## Expected Outcome

* The service restarts automatically
* No manual restart is required
* systemd handles recovery

---

## Step 4: Reflection

Answer these questions:

* What component detected the failure?
* What action was automated?
* How would this scale to 100 servers?

---

## Optional Extension

Think about how this changes in cloud-native environments:

* Auto Scaling Groups
* Kubernetes liveness probes
* GitOps reconciliation

---

## Key Learning

> If recovery requires a human, automation is incomplete.


Just tell me 👍
```
