# Canary Deployment Strategy

Canary deployment ek progressive deployment strategy hai jisme application ka naya version kuch percent users ke liye deploy kiya jata hai.

Isme Blue-Green strategy jesa server setup hota hai, jaise 1 server par purana application chal rha hota hai aur dusre server par naya application deploy kiya jata hai aur uspe kuch users ko hi access diya jata hai.

“Canary deployment ek aisi strategy hai jisme hum naya version sabko ek saath nahi dete, balki pehle sirf thode users ko dete hain, fir dheere-dheere sabko.”

Matlab tum app ke naye version ko deploy karte ho, lekin initially sirf 1%, 5%, ya 10% traffic us version par jaata hai. Agar sab kuch theek chalta hai, to gradually 100% traffic nayi version par divert kar dete ho.

Canary Deployment ek progressive delivery strategy hai jisme hum naya version limited users ke liye rollout karte hain,monitor karte hain, aur gradually sab users tak rollout kar dete hain — agar koi problem aaye to turant rollback karte hain — bina downtime aur bina production impact ke.

<br>

<img src="https://drive.google.com/uc?export=view&id=1-YbBLPH4Y-CZta0edBxKGNCAS4HfA2VI" width="450" height="410">

<br>

<img src="https://drive.google.com/uc?export=view&id=1gWGXY2muPRIpf14h-slZ9gFR0hOa37go" width="450" height="410">

<br>

<img src="https://drive.google.com/uc?export=view&id=109KQoct41nTh5ayHQgCnrSOQbQSFhGZZ" width="450" height="410">

Explanation:
- Jaise uper example mein two servers (Environment) bane hue hain, deployment se pehle 100% traffic server-1 pe ja rha hai.
- Dusre server par application ke naye version ki deployment hui, deployment ke baad 2% percent traffic server-2 pe ja rha hai.
- Ese mein application ke naye version ki testing ki jati hai, agar testing mein sab kuch thik rehta hai hai to 100% traffic ko server-2 yani naye deployment pe shift kar dete hain.

<br>
<br>

### Naam “Canary” Kyun Pada?

Ye concept mining industry se aaya hai ⛏️

Pehle miners apne saath ek canary bird le jaate the mine mein. Agar poisonous gas leak hoti thi to bird sabse pehle react karta tha — isse miners ko warning mil jaati thi ki danger hai.

Software world mein bhi same idea hai 🧠

“Naye version ko pehle limited users (canaries) ko dete hain — agar koi issue aaya, to deployment stop kar dete hain.”

Isliye isse Canary Deployment kaha gaya.

<br>
<br>

### Why Use Canary Deployment?

Canary strategy ka main goal hota hai:
- **Risk reduction** — early detection of issues (Isme issue pehle hi detect ho jata hai, kyuki kuch users ko testing purpose ke liye access dete hain).
- **Zero downtime**.
- **Gradual rollout**.
- **Instant rollback if issues found**.

<br>
<br>

### Basic Architecture Diagram

```
               +-----------------------------+
               |        Load Balancer        |
               +---------------+-------------+
                               |
         ---------------------------------------------
         |                       |                   |
     [Old Version v1]       [New Version v2]   [New Version v2]
      (90% traffic)           (5% traffic)       (5% traffic)

```

Initially — 90% users old version par, 5–10% new version par test ho raha hai.

Agar sab theek, to ye ratio gradually shift hota hai → 70/30 → 50/50 → 0/100.

<br>
<br>

### Canary Deployment in Real World (Automation)

Manual scaling ka process tedious hota hai. Industry mein Canary rollout mostly automated hota hai — tools like:
- Argo Rollouts (Kubernetes-native progressive delivery controller)
- Flagger (FluxCD ecosystem)
- Spinnaker
- Istio / Linkerd (Service mesh based canary routing)
- AWS CodeDeploy Canary strategy
- Google Cloud Deploy Canary steps

<br>
<br>

### Disadvantages / Trade-offs

| Limitation                        | Description                         |
| --------------------------------- | ----------------------------------- |
| ⚠️ **Complex Routing**            | Needs weighted load balancing       |
| ⚠️ **More Management**            | Two versions co-exist for long time |
| ⚠️ **Monitoring Required**        | Must track metrics actively         |
| ⚠️ **Session Consistency Issues** | If users flip between versions      |
| ⚠️ **Automation Needed**          | Manual scaling inefficient          |

<br>
<br>

### Canary vs Blue-Green vs Rolling Update

| Feature             | Canary                | Blue-Green                | Rolling Update      |
| ------------------- | --------------------- | ------------------------- | ------------------- |
| **Traffic Split**   | Gradual % (weighted)  | Binary switch             | Gradual pod replace |
| **Rollback**        | Instant (stop canary) | Instant (switch env)      | Gradual rollback    |
| **Downtime**        | ❌ No                  | ❌ No                      | ❌ No                |
| **Traffic Control** | Fine-grained          | Full switch               | None (automated)    |
| **Infra Cost**      | Medium                | High                      | Low                 |
| **Ideal For**       | Risk-sensitive apps   | High-availability systems | Regular releases    |
