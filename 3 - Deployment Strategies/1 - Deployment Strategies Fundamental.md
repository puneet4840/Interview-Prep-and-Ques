# Deployment Strategies Fundamentals

<br>

### Pehle Samjho — Deployment Kya Hoti Hai?

Deployment ka matlab hai application ke naye version ko purane version se badalna. Matlab applicationn ko naye version mein upgrade karna.

Application ke naye changes ko live karna.

Application ko deploy karne ke kayi sare methods hote hain.

<br>
<br>

### Deployment Strategy Kya Hoti Hai?

“Deployment Strategy” ka matlab hota hai:
- Koi bhi application ke naye version ko existing running version ke upar kaise aur kis tarike se deploy kiya jaaye.

Matlab simple words mein:
- "Tum apne running application ko update karte ho bina users ko problem diye — uske process ko hum Deployment Strategy bolte hain."

Software mein jab tum app ka naya version deploy karte ho (new code, new image, new configuration), tab tumhe decide karna padta hai:
- Kya purana version turant band karna hai?
- Kya dono version kuch time tak parallel chalenge?
- Kya naye version ko thoda thoda traffic dena hai test karne ke liye?

Ye sab Deployment Strategy ke part hote hain.

<br>
<br>

### Common Deployment Strategies

Chalo sabse pehle dekhte hain 5 common deployment strategies jo IT industry mein use hoti hain — Kubernetes ho ya AWS EC2, ECS, Jenkins, Ansible, ya Azure pipelines — concept same hota hai.

| Strategy                  | Description                                                                   | Downtime | Risk        | Parallel Versions |
| ------------------------- | ----------------------------------------------------------------------------- | -------- | ----------- | ----------------- |
| **Recreate**              | Purane version ko delete karke naya deploy karna                              | ✅ Yes    | 🔺 High     | ❌ No              |
| **Rolling Update**        | Gradually old pods ko replace karna                                           | ❌ No     | 🟢 Low      | ✅ Yes             |
| **Blue-Green Deployment** | Purana (blue) aur naya (green) version dono parallel, traffic switch hota hai | ❌ No     | 🟢 Low      | ✅ Yes             |
| **Canary Deployment**     | Thoda traffic nayi version ko bhejna (testing ke liye)                        | ❌ No     | 🟢 Very Low | ✅ Yes             |
| **A/B Testing**           | Alag alag user groups ko alag version dena                                    | ❌ No     | 🟢 Medium   | ✅ Yes             |


In sab strategies ko aage samjhenge.
