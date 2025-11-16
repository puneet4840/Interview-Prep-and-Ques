# Blue-Green deployment strategy

<br>

### Blue-Green Deployment Hota Kya Hai?

Blue-Green Deployment ek zero downtime deployment strategy hai jisme hum application ke do alag environments banate hain:
- Blue Environment → Current live version (jo abhi users use kar rahe hain).
- Green Environment → New updated version (jo tum deploy kar rahe ho).

Blue-Green strategy mein 2 envrionments hote hain, ek Blue Environment jispar current application chal rha hota hai aur 100% traffic isi environment par jata hai. Dusra environment hota hai Green, humko application ka naya version green environment par deploy karna hota hai, fir testing karke Blue se Green Environment ko swap kar dete hain matlab blue pe jane wale traffic ko green par bhej dete hain, 100% traffic fir green environment par jata hai.

Isi ko blue green deployment strategy kehte hain.

<img src="https://drive.google.com/uc?export=view&id=1TTsJm_d2TwJTY9b_33uqHtlLcMlXxudA" width="650" height="560">

Aur agar koi problem aa jaaye, Bas traffic wapas Green se Blue par switch kar do — bina downtime ke.

Blue-Green ek pair of production environments hota hai jisme ek active hai (Blue ya Green) aur doosra idle/standby hota hai.

<br>
<br>

### Blue-Green Deployment Ka Objective

- Zero Downtime.
- Instant Rollback.
- Safe Production Deployment.
- Parallel Testing of New Version.

<br>
<br>

### Step-by-Step Lifecycle of Blue-Green Deployment

**Step 1: Blue Environment (Old Live Version)**:
- Tumhare paas ek application chal rahi hai (for example: nginx:1.14).
- Ye environment tumhara production hai jahan live traffic aa raha hai.
- Load balancer (ya service) Blue environment ke pods/servers par route karta hai.

<br>

**Step 2: Deploy New Version (Green)**:
- Tum ek completely separate deployment banate ho same configuration ke saath, bas image (version) alag hoti hai.
- Green environment ek mirror copy hota hai Blue ka (same CPU, memory, ports, readiness checks, etc.).

<br>

**Step 3: Validation & Testing**:
- Ab tum internally test kar sakte ho ki Green environment properly kaam kar raha hai ya nahi.
- Functional testing, performance testing, health checks sab Green environment pe hote hain.

<br>

**Step 4: Switch Traffic (Cutover)**:
- Jaise hi testing complete ho jaaye aur Green 100% ready ho, tum sirf service selector (ya load balancer target group) change karte ho aur traffic green environment pe chla jata hai.
- Ab se saara incoming traffic Green environment ke pods par jaata hai, Blue ab idle ho gaya.

Traffic switch time: milliseconds

Downtime: zero

<br>

**Step 5: Monitor New Version**:
- Green deployment monitor karo: logs, latency, CPU, error rate, response time, etc.
- Ye stage crucial hai — agar sab kuch smooth chal raha hai, Blue ko decommission kar sakte ho.

<br>

**Step 6: Rollback (If Issue Found)**:
- Agar Green mein koi bug, crash ya performance issue milta hai:
  - Tumhe purani state restore karne ki zarurat nahi, sirf service selector wapas Blue environment par point kar do.

Rollback done ✅, Aur users ko pata bhi nahi chala!

<br>

**Step 7: Clean Up or Promote**:
- Agar Green environment stable chal raha hai, to tum Blue ko delete kar sakte ho (ya backup ke liye rakh sakte ho).
- Next release mein Green “old” ban jaata hai aur naya version fir “Blue” environment mein aata hai.
- Yani Blue aur Green ka role flip hota rehta hai har release ke saath.

<br>
<br>

### Limitations and Trade-offs

| Limitation                      | Explanation                                            |
| ------------------------------- | ------------------------------------------------------ |
| ⚠️ **Resource Cost**            | Double infra (Blue + Green) required                   |
| ⚠️ **Database Synchronization** | DB schema/version must be compatible for both versions |
| ⚠️ **Traffic Switching Risk**   | LB misconfiguration can cause route mix                |
| ⚠️ **Automation Needed**        | Manual switching prone to error                        |
| ⚠️ **CI/CD Integration**        | Needs well-defined pipeline logic                      |
