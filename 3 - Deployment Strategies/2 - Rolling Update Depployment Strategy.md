# Rolling Update Deployment Strategy

<br>

### Pehle Basic Samjho — Rolling Update Hoti Kya Hai?

Rolling update ek deployment strategy hai jo Kubernetes mein use hoti hai taaki application ko update karte waqt zero downtime ke saath naye pods deploy kiye ja sakein.

Kaise kaam karti hai:
- Rolling update deployment strategy mein pehle naye pods ek-ek karke create hote hain fir purane pod ek-ek karke terminate hote hain.

Isme purne pods ko naye pods ke saath swap kiya jata hai.

So basically tumhare users ko application downtime feel nahi hota.

Example:
- Suppose kubernetes mein tumhare koi application chal rha hai aur uske 3 pods running hain. Tumne rolling update strategy se deployment ki. To isme pehle ek-ek karke 3 naye pods create ho jayenge fir 3 purane pods ek-ek karke delete ho jayenge.

- Isliye is strategy mein zero-downtime hota hai, user ko application hamesha running hi milegi.

<br>
<br>

### Kyu Use Karte Hain Rolling Update?

Imagine karo tumhare paas ek application hai jiska deployment version ```v1``` chal raha hai (for example: ```nginx:1.14``` image). Ab tumhe usse upgrade karna hai ```v2``` (```nginx:1.16```) par.

Agar tum "Recreate Strategy" use karoge to kya hoga?
- Pehle purane saare pods delete ho jayenge.
- Fir naye pods create honge.
- Iss dauran app down rahegi.

Rolling Update is better because:
| Strategy      | Downtime | Description                                                    |
| ------------- | -------- | -------------------------------------------------------------- |
| Recreate      | ❌ Yes    | Purane pods delete hone ke baad naye pods aate hain            |
| RollingUpdate | ✅ No     | Purane pods dheere-dheere replace hote hain naye pods ke saath |

<br>
<br>

### Rolling Update Kubernetes Mein Kaise Kaam Karta Hai?

- Deployment YAML file mein tum apne app ka configuration define karte ho, jaise replica count, container image, aur strategy type.
- Jab tum ```kubectl apply -f deployment.yaml``` run karte ho, to Kubernetes ek Deployment object create karta hai.
- Deployment internally ek ReplicaSet create karta hai jo actual Pods ko manage karta hai.
- Jab tum update karte ho (for example image version change karte ho), to:
  - Kubernetes ek naya ReplicaSet create karta hai naye version ke liye.
  - Dheere-dheere purane ReplicaSet ke pods delete hote hain aur naye ReplicaSet ke pods create hote hain.

Ye process controller manager ke through chalti hai jiska role hota hai desired aur current state ko match karwana.

<br>
<br>

### Rolling Update sirf Kubernetes mein use hoti hai ya kahin aur bhi?

Nahi, ye sirf Kubernetes tak limited nahi hai.

Rolling Update ek general deployment concept hai jo multiple platforms aur tools mein use hota hai — lekin Kubernetes ne isko automate aur standardize kar diya hai.

**Rolling Update Use Cases Beyond Kubernetes**:

**Docker Swarm**:
- Docker Swarm mein bhi tum ```docker service update``` command se rolling updates kar sakte ho.

<br>

**AWS Elastic Beanstalk**:
- AWS Elastic Beanstalk mein bhi rolling update ka concept hai.
- Jab tum naya version deploy karte ho, Beanstalk EC2 instances ko batch-wise update karta hai.

<br>

**Azure App Service Deployment**:
- Azure App Services ke paas rolling deployment option hai jisme ye slots ka use karta hai.
- Tum staging slot mein naya version deploy karte ho, test karte ho, aur fir swap karte ho.
- Isme bhi zero downtime aur gradual rollout possible hai.

<br>

**Jenkins / Ansible Pipelines**:
- CI/CD tools jaise Jenkins, Ansible, GitHub Actions mein bhi tum rolling update implement kar sakte ho.

<br>
<br>

### Rolling Update Mein Important Parameters (Concepts)

Kubernetes mein rolling update ke behavior ko fine-tune karne ke liye kuch important fields hoti hain:

```maxUnavailable```:
- Ye define karta hai ki update ke time kitne pods temporarily unavailable ho sakte hain.
- Ye number (e.g., 1) ya percentage (e.g., 25%) dono ho sakta hai.

Example:

Agar tumhare paas 4 replicas hain aur ```maxUnavailable: 1``` hai, to update ke dauran 1 pod tak unavailable ho sakta hai.

Matlab at least 3 pods hamesha available rahenge.

<br>

```maxSurge```:
- Ye define karta hai ki update ke time kitne extra pods temporarily create kiye ja sakte hain desired count se zyada.
- Ye bhi number ya percentage ho sakta hai.

Example:
Agar replicas = 4 aur ```maxSurge: 1``` hai, to update ke dauran ek extra pod create ho sakta hai (total 5 temporary).

<br>
<br>

### Rolling Update YAML Example

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  replicas: 4
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14
        ports:
        - containerPort: 80
```

Ab jab tum image version change kar doge:
```
kubectl set image deployment/my-nginx nginx=nginx:1.16
```

Kubernetes rolling update process start karega.

<br>
<br>

### Step-by-Step Rolling Update Process Example

Maan lo tumhare paas total 4 replicas hain (replicas: 4), aur tumne ```maxUnavailable: 1``` aur ```maxSurge: 1``` set kiya hai.

| Step | Action             | Old Pods (v1) | New Pods (v2) | Total Pods | Description                          |
| ---- | ------------------ | ------------- | ------------- | ---------- | ------------------------------------ |
| 1    | Start              | 4             | 0             | 4          | Initial state (v1 running)           |
| 2    | Create new pod     | 4             | 1             | 5          | maxSurge=1 allows 1 extra pod        |
| 3    | Delete one old pod | 3             | 1             | 4          | maxUnavailable=1 — 1 old pod deleted |
| 4    | New pod ready      | 3             | 2             | 5          | Next surge again                     |
| 5    | Delete old pod     | 2             | 2             | 4          | Replace continues                    |
| 6    | Repeat process     | 1             | 3             | 4–5        | ...                                  |
| 7    | All updated        | 0             | 4             | 4          | Done ✅                               |


<br>
<br>

### Rolling Update Monitor Kaise Karte Hain?

Tum rolling update ka status check kar sakte ho:
```
kubectl rollout status deployment/my-nginx
```
Output kuch aisa aayega:
```
Waiting for rollout to finish: 2 out of 4 new replicas have been updated...
deployment "my-nginx" successfully rolled out
```

Agar tum rollback karna chahte ho (naye version mein issue hai), use:
```
kubectl rollout undo deployment/my-nginx
```

