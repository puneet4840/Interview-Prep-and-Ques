# Interview Question: You have deployed an app to Azure Kubernetes Service and it fails health checks randomly. How do you debug this end-to-end?

<br>

### Sabse Pehle: Health Check Samjho

Kubernetes mein mainly 3 types ke probes hote hain:
- Liveness Probe:
  - App dead hai ya stuck hai?
    
- Readiness Probe:
  - App traffic lene ke liye ready hai ya nahi?
    
- Startup Probe:
  - Slow startup apps ke liye.
 
Agar health checks randomly fail ho rahe hain, iska matlab ho sakta hai:
- App crash ho raha hai
- CPU/memory pressure hai
- DB timeout aa raha hai
- GC pause ho raha hai
- Network issue hai
- DNS issue hai
- Probe configuration galat hai
- Node issue hai
- Azure Load Balancer issue hai
- Ingress issue hai
- Dependency issue hai

<br>
<br>

### Step-by-Step End-to-End Debugging

**STEP 1 — Symptoms Understand Karna**:

Sabse pehle mai ye identify karunga:

Kaunsi probe fail ho rahi hai?
- Liveness?
- Readiness?
- Startup?

Randomly fail ka matlab:
- Specific time?
- High traffic?
- Deployment ke baad?
- Node specific?
- Region specific?

Impact kya hai?
- Pod restart?
- Traffic drop?
- 502/503?
- Slow response?

<br>

**STEP 2 — Pod Status Check**:

Sabse pehle pod state dekhenge.
```
kubectl get pods -n app-ns
```

Agar restart ho raha hai:
```
kubectl get pods -n app-ns -o wide
```

Ye important hai kyunki:
- same node pe issue ho sakta hai.
- specific node unhealthy ho sakta hai.

<br>

**STEP 3 — Describe Pod**:

Most important command:
```
kubectl describe pod <pod-name> -n app-ns
```
Yaha mujhe events milenge:

Example:
```
Readiness probe failed: HTTP probe failed with statuscode: 500
Liveness probe failed: timeout
Back-off restarting failed container
```

Isse immediately direction milti hai.

<br>

**STEP 4 — Probe Configuration Analyze**:

Fir mai deployment yaml check karunga.
```
kubectl get deploy app -o yaml
```

Yaha especially ye fields dekhunga:
```
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 5
  timeoutSeconds: 1
  periodSeconds: 5
  failureThreshold: 3
```

<br>

**STEP 5 — Container Logs**:

Ab logs check karunga.
```
kubectl logs <pod>
```
Agar restart hua:
```
kubectl logs <pod> --previous
```
Ye bahut important hai.

