---
title: "Alive, Ready, or Just Existing: Kubernetes Probes Explained"
categories:
  - Blog
tags:
  - Kubernetes
  - DevOps
  - Containers
  - Cloud Native
---

I recently started learning Kubernetes, and one of the concepts I came across was **probes**.

At first, names like **startup probe**, **readiness probe**, and **liveness probe** felt confusing.
But once I mapped each one to a simple question, everything made sense.

In simple terms:

- **Startup probe** asks: “Have you finished starting?”
- **Readiness probe** asks: “Are you ready to handle traffic?”
- **Liveness probe** asks: “Are you still healthy?”

These checks help Kubernetes decide when to send traffic and when to restart a stuck container.

---

## Why Probes Matter

A running process does not always mean a healthy application.

Common cases:

- The application might still be starting up
- The database connection may not be ready
- An external dependency might be unavailable
- The process could be stuck in a deadlock
- The application might be returning errors

Without probes:

- Kubernetes may send traffic to a pod that is not ready
- A hung application may keep running forever
- Users start seeing errors even though the pod looks "Running"

Probes give Kubernetes a better view of what is actually happening inside your app.

---

## 1. Startup Probe

The startup probe answers:
> "Has the application finished starting?"

This is useful for slow-starting applications.  
Without it, Kubernetes may restart a pod before startup is complete.

### Example

```yaml
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  periodSeconds: 10
  failureThreshold: 30
```

Here, Kubernetes checks `/startup` every 10 seconds and allows 30 failures.
That gives the app up to 300 seconds to boot.

---

## 2. Readiness Probe

The readiness probe answers:
> "Can this pod receive traffic right now?"

If this probe fails, the pod is removed from Service endpoints.
Traffic stops, but the container is **not restarted**.

### Example

```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  periodSeconds: 5
  failureThreshold: 3
```

This is useful when your app depends on:

- Databases
- External APIs
- Caches
- Background initialization

A pod can be running and still be temporarily unready.

---

## 3. Liveness Probe

The liveness probe answers:
> "Is the application still healthy?"

If this probe keeps failing, Kubernetes restarts the container.

### Example

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  periodSeconds: 10
  failureThreshold: 3
```

This helps recover from stuck states such as:

- Deadlocks
- Infinite loops
- Hung threads
- Unresponsive processes

Kubernetes handles this restart automatically.

---

## How They Work Together

Typical flow:

1. The container starts
2. Startup probe checks initialization
3. The readiness probe determines when the pod can receive traffic
4. The liveness probe keeps checking that the application remains healthy

Each probe solves a different problem.

| Probe | Question | What Happens on Failure |
|------|------|------|
| Startup | Has the app started? | Kubernetes keeps waiting or eventually restarts the container |
| Readiness | Can it receive traffic? | Pod is removed from Service endpoints |
| Liveness | Is it still healthy? | Container is restarted |

---

## Can You Use All Three?

Yes, and it is common in production.
They are not alternatives; each one handles a different stage.

- **Startup probe** gives slow applications enough time to boot
- **Readiness probe** ensures traffic goes only to ready pods
- **Liveness probe** restarts the application if it gets stuck later

For many real-world apps, this combination is the safest default.

---

## Complete Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app
          image: my-app:latest
          ports:
            - containerPort: 8080

          startupProbe:
            httpGet:
              path: /startup
              port: 8080
            periodSeconds: 10
            failureThreshold: 30

          readinessProbe:
            httpGet:
              path: /ready
              port: 8080
            periodSeconds: 5
            failureThreshold: 3

          livenessProbe:
            httpGet:
              path: /health
              port: 8080
            periodSeconds: 10
            failureThreshold: 3
```

---

## Final Thoughts

Kubernetes probes look complex at first, but the idea is straightforward:

- Startup probe protects slow boot
- Readiness probe protects user traffic
- Liveness probe protects long-running health

Together, they prevent one of the most common production issues:
a pod that is technically running but not actually usable.

---

## Over to You

How are you using probes in your applications?  
Do you usually configure all three, or only readiness and liveness?
