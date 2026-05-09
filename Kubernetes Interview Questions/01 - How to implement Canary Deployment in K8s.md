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

