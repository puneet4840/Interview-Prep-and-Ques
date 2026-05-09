# How to implement Canary Deployment in Kubernetes?

<br>

### Kubernetes Mein Default Deployment Method Kya Hai?

Jab tum normal Deployment update karte ho (```kubectl apply -f deployment.yaml```) aur Kubernetes dheere-dheere old pods ko replace karta hai new pods se, usko generally:

**Rolling Update Deployment** kehte hain.

Kubernetes Deployment ka default deployment strategy bhi यही होती hai.

Rolling Update Deployment mein:
- Ek naya pod create karega.
- Health check pass hone ka wait karega.
- Ek old pod delete karega
- Fir next new pod create karega
- Ye process repeat hoti hai jabtak desired state maintain na ho jaye.

Rolling update mein:
- Saare users eventually new version par chale jate hain.
- Gradual replacement hoti hai.
- Traffic control nahi hota.
- Percentage-based testing nahi hoti

Isi limitation ko solve karta hai: **Canary Deployment**.

<br>
<br>

### Canary Deployment Kya Hoti Hai?

Canary deployment mein application ka naya version sirf kuch percent users ko dikhya jata hai baaki users old old stable version ko hi use karte hain.

Agar new version stable nikla to dheere-dheere traffic badha dete hain.

Agar issue aya to quickly rollback kar dete hain.

<br>
<br>

### Kubernetes Mein Canary Deployment Kaise Karte Hain?

Kubernets mein built-in canaray deployment karne ka koi method nahi hota hai. 

Kubernetes mein by-default rolling update deployment method hota hai.

Kubernetes provide karta hai:
- Deployments
- Services
- Ingress
- Networking primitives

Inko combine karke: Canary behavior build kiya jata hai.

Kubernetes mein canary deployment acheive karne ki Generally 3 approaches use hoti hain:

| Method                         | Difficulty | Real Industry Usage |
| ------------------------------ | ---------- | ------------------- |
| Multiple Deployments + One Service | Easy       | Basic               |
| Ingress-based Canary           | Medium     | Very common         |
| Service Mesh (Istio/Linkerd)   | Advanced   | Enterprise          |

<br>
<br>

### Method-1: Multiple Deployments + One Service

Is method mein ek to old stable deployment version pehle se hi chal rha hota hai, hum ek nayi deployment banate hain jisme same labels dete hain jo old deployment mein diye the. Aur ek hi service dono deployment ke pods pe traffic bhejti hai.

Matlab Hum:
- v1 deployment alag banate hain
- v2 deployment alag banate hain

Aur same service dono ko traffic bhejti hai.

Architecture:
```
                Service
                   |
      --------------------------------
      |                              |
   Deployment-v1                Deployment-v2

```

<br>

**Ye Kaam Kaise Karta Hai?**:

Service label selector use karti hai traffic pod pe bhejne ke liye:

Example:
```
selector:
  app: myapp
```
Matlab:
- Jinke pods ke paas ```app=myapp``` label hai un pods ko traffic bhejo.

**Ab Clever Trick**:

Hum:
- v1 deployment
- v2 deployment

dono mein same label dete hain:
```
app: myapp
```

Result: Service dono ko traffic bhejne lagti hai.

<br>

**Example**:

**v1 Deployment**:
```
apiVersion: apps/v1
kind: Deployment

metadata:
  name: myapp-v1

spec:
  replicas: 9

  selector:
    matchLabels:
      app: myapp
      version: v1

  template:
    metadata:
      labels:
        app: myapp
        version: v1
```

Ye deployment Kya Kar Raha Hai? 9 stable pods create.

**v2 Deployment**:
```
apiVersion: apps/v1
kind: Deployment

metadata:
  name: myapp-v2

spec:
  replicas: 1

  selector:
    matchLabels:
      app: myapp
      version: v2

  template:
    metadata:
      labels:
        app: myapp
        version: v2
```

Ye deployement Kya Kar Raha Hai?

Sirf:
- 1 canary pod create.

**Service**:
```
apiVersion: v1
kind: Service

metadata:
  name: myapp-service

spec:
  selector:
    app: myapp
```

Ab Internally Kya Hua?

Service ne dekha:
```
app=myapp
```
Matching pods:
```
9 v1 pods
1 v2 pod
```
Service dono deployments ke pods ko select karti hai labels ke basis par

Result

Approx:
```
90% → v1
10% → v2
```
Kyuki Kubernetes Service round-robin/load-balancing karta hai.



**Rollback**:

Agar bug aya:
```
kubectl scale deployment myapp-v2 --replicas=0
```
Traffic instantly v1 par chali jayegi.

**Important Understanding**:

Yaha Kubernetes officially:
- Canary nahi kar raha.
- Bas load balancing kar raha.

Hum canary behavior create kar rahe hain.


<br>

**Disadvantages**:

1 - Sabse pehla disadvantage ye hai ki traffic distribution accurate nahi hota. Hum generally sochte hain ki agar old version ke 9 pods aur new version ka 1 pod hai, to 10% traffic automatically new version par jayega. Lekin Kubernetes service actual request percentage ko control nahi karti. Ye sirf available pods ke beech traffic distribute karti hai. Isliye kabhi 5% traffic new version par jata hai aur kabhi 20%, especially jab keep-alive connections, uneven load, ya slow pods involved hote hain.

2 - Dusra issue session affinity ka hota hai. Agar application stateful hai, jaise login sessions, carts, ya websocket connections, to kuch users repeatedly same pod par hi request bhejte rehte hain. Is situation mein kuch users hamesha canary version use karenge aur kuch users kabhi canary hit hi nahi karenge. Isse proper testing aur real traffic analysis difficult ho jata hai.

3 - Teesra disadvantage ye hai ki Kubernetes service intelligent routing provide nahi karti. Ye sirf labels ke basis par traffic bhejti hai. Isliye hum specific users, headers, cookies, locations, ya devices ke basis par traffic control nahi kar sakte. Example ke liye, agar hum chahte hain ki sirf internal employees new version use karein, to simple Kubernetes service se ye possible nahi hota.

Isliye Industry Kya Use Karti Hai?
**Ingress-Based Canary** ya **Service Mesh**.

<br>
<br>

### Ingress-Based Canary Deployment

Kubernetes mein ingress traffic ko route karta hai services par host aur path ke based par.

Ingress based canary deployment Kubernetes mein ek bahut popular aur production-grade approach hai, jahan hum traffic ko directly ingress layer par control karte hain instead of sirf Kubernetes Service par depend rehne ke.

Is method mein generally:
- ek stable deployment hoti hai (v1)
- ek canary deployment hoti hai (v2)
- aur ingress controller decide karta hai ki kitna traffic kis version ko bhejna hai.

<br>

**Basic Idea**:

Ingress-based canary mein hum do ingress resource use karte hain, ek normal ingress jo stable version ke service pe traffic behjta hai aur dusra ingress resource canary service pe traffic bhejta hai. Canary ingress resource mein hum annotations ka use karte ho jo actual canary deployment acheive karta hai.

Architecture kuch aisa hota hai:
```
Users
   ↓
Ingress Controller
   ↓
-------------------------
| Stable Service (v1)   |
| Canary Service (v2)   |
-------------------------
```

Ingress controller:
- traffic split karta hai
- headers inspect kar sakta hai
- cookies check kar sakta hai
- percentage routing kar sakta hai

Ye Layer 7 (HTTP level) par kaam karta hai.

**Components**:

Ingress-based canary deployment mein generally ye components hote hain:
- Stable Deployment
- Canary Deployment
- Stable Service
- Canary Service
- Ingress Controller
- Canary Ingress Rules

<br>

**Step-by-Step Example**:

Suppose tumhari application ka naam hai:
```
my-app
```
Ab tum:
- ```v1``` ko stable version rakhna chahte ho.
- ```v2``` ko canary version banana chahte ho.

**Step 1: Stable Deployment**:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-v1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
      version: v1
  template:
    metadata:
      labels:
        app: my-app
        version: v1
    spec:
      containers:
      - name: app
        image: my-app:v1
```

**Step 2: Stable Service**:
```
apiVersion: v1
kind: Service
metadata:
  name: my-app-stable
spec:
  selector:
    app: my-app
    version: v1
  ports:
  - port: 80
```

**Step 3: Canary Deployment**:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-v2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
      version: v2
  template:
    metadata:
      labels:
        app: my-app
        version: v2
    spec:
      containers:
      - name: app
        image: my-app:v2
    targetPort: 8080
```

**Step 4: Canary Service**:
```
apiVersion: v1
kind: Service
metadata:
  name: my-app-canary
spec:
  selector:
    app: my-app
    version: v2
  ports:
  - port: 80
    targetPort: 8080
```

**Step 5: Main Ingress**:

Ab normal traffic stable version par jayega.

Example using NGINX Ingress Controller:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app-main
spec:
  ingressClassName: nginx
  rules:
  - host: myapp.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-app-stable
            port:
              number: 80
```

**Step 6: Canary Ingress**:

Ab canary ingress define karte hain.

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app-canary
  annotations:
    nginx.ingress.kubernetes.io/canary: "true"
    nginx.ingress.kubernetes.io/canary-weight: "10"
spec:
  ingressClassName: nginx
  rules:
  - host: myapp.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-app-canary
            port:
              number: 80
```

Ab Kya Hoga?

Ingress controller automatically:
```
90% traffic → stable service
10% traffic → canary service
```
bhej dega. Aur ye pod-count based nahi hota. Ye actual weighted routing hoti hai.

<br>

**Header-Based Canary**:

Bahut powerful feature hai.

Suppose tum chahte ho Sirf QA team canary use kare.

To ingress headers inspect kar sakta hai.

Example:
```
annotations:
  nginx.ingress.kubernetes.io/canary: "true"
  nginx.ingress.kubernetes.io/canary-by-header: "X-Canary"
  nginx.ingress.kubernetes.io/canary-by-header-value: "true"
```

Agar request mein header ho:
```
X-Canary: true
```
to request canary deployment par jayegi. Otherwise stable version par.

Example

QA engineer:
```
curl -H "X-Canary: true" https://myapp.com
```
Normal users Automatically stable version use karenge.

<br>

**Cookie-Based Canary**:

Ingress cookies bhi inspect kar sakta hai.

Example:
```
nginx.ingress.kubernetes.io/canary-by-cookie: "canary"
```
Ab:
```
Cookie: canary=always
```
wale users canary version use karenge. Ye A/B testing mein bahut useful hota hai.

<br>
<br>

**Lekin ingress resource humne do kyu banaye kya ingress multiple ingress resource accept kar sakta hai? Kya ek hi ingress resource se kaam nhi ho sakta, Agar haan to kaise hoga vo?**

Haan, Kubernetes mein ek ingress controller multiple Ingress resources ko handle kar sakta hai. Jab hum canary deployment mein do ingress resources banate hain:
- ek normal ingress.
- ek canary ingress.

to actually dono same host aur same path ke liye kaam kar rahe hote hain. Ingress controller internally in dono configurations ko merge karta hai.

**Example**:
```
Main Ingress
Host: myapp.com
Path: /
→ stable service
```
Aur:
```
Canary Ingress
Host: myapp.com
Path: /
→ canary service
```
Ingress controller dono ko combine karke ek final routing configuration banata hai.

<br>
<br>

**Request Flow End-to-End**:

Ab actual request flow samjho.

Suppose user request bhejta hai:
```
https://myapp.com
```

Step 1: DNS request ingress load balancer tak jati hai.
```
myapp.com → ingress external IP
```

Step 2: Ingress controller request receive karta hai.

Step 3: Ingress controller internally check karta hai:
```
Kya canary rules exist karti hain?
```
Agar haan:
- weight check karo
- header check karo
- cookie check karo


Step 4: Suppose canary-weight = 10 hai.

Controller internally randomization logic use karta hai.

Pseudo logic:
```
if random(1-100) <= 10:
    send to canary
else:
    send to stable
```

Step 5: Request selected service ko forward hoti hai.
```
Stable service → v1 pods
Canary service → v2 pods
```

