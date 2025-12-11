# Terraform Interview Questions (Part-1)

## Que-1: What is the difference between terraform import and terraform taint?

Iska simple answer ye hai:

- Terraform import ka use hota hai kisi already existing cloud resource ko Terraform ke control me lane ke liye.
  - Matlab koi resource pehle se cloud mein bana hua hai, kisi ne manually bana diya, lekin terraform ki state file mein us resource ki configuration nhi hai, to terraform ko nhi pata hoga us resource ke baare mein, to ```terraform import``` us resource ko state file mein configure karta hai jisse vo resource terraform ke control mein aa jata hai.
 
- Terraform taint ka use hota hai kisi existing Terraform se create hue resource ko forcefully destroy karke fir se recreate karne ke liye.
  - Matlab terraform ne cloud mein koi resource create kiya hai, lekin suppose vo resource corrupt ho gya ya koi issue aa gya ya resource ko manually change kar diya ho, ```terraform taint``` us resouce ko destroy karke fir se create karta hai.
 
Lekin ab chalo isko andar ki depth, state file, lifecycle, aur real example ke saath samajhte hain.

<br>
<br>

### Terraform Import

Terraform import ka use hum tab karte hain jab koi resource pehle se cloud me manually ya kisi aur tool se bana hua hota hai, aur hume usko Terraform ke state file me bring karke infrastructure-as-code ke through manage karna hota hai.

**Real-Life Example Samjho**:

Socho:
- Tumne AWS Console se manually ek EC2 instance bana diya.
- Baad me tumhari company boli:
  - “Ab se sab kuch Terraform se manage hoga”.

Problem:
- Terraform ko pata hi nahi hai ki wo EC2 instance exist karta hai.
- Agar tum directly ```terraform apply``` kar do:
- Terraform bolega:
  - “EC2 exist nahi karta, main naya bana deta hoon”
  - Result: Duplicate EC2 instance ban jayega.
 
Yahin pe aata hai Terraform Import.

**Terraform Import Actual Kaam Kya Karta Hai?**

Terraform import existing resource ko terraform state file (```terraform.tfstate```) ke andar add karta hai, Matlab ab Terraform bolega “Haan, ye resource mere control me hai”.

Terraform import:
- ```.tf``` file automatically generate nahi karta.
- Tumhe pehle se resource ka block likhna padta hai.

<br>

**Flow Samjho Step-by-Step**:
- Tum ```.tf``` file me resource likhte ho.
- ```terraform import``` chalate ho.
- Terraform:
  - AWS se live configuration fetch karta hai, State file me inject karta hai, Ab tum terraform plan karte ho, Terraform compare karta hai:
  - ```.tf``` vs real infra.
 
<br>
<br>

### Terraform Taint

Terraform taint ka use hum tab karte hain jab koi resource Terraform ke state me already exist karta hai, matlab terraform ne resource pehle se hi create kar rakha hai, lekin hum usko forcefully destroy & recreate karna chahte hain — chahe configuration change hua ho ya nahi.

**Real-Life Example**:

Socho:
- Tumhara EC2 instance perfectly Terraform se created hai.
- Lekin:
  - Disk corrupted ho gaya.
  - OS crash ho gaya.
  - Manual changes ho gaye.
  - Ya tumhe doubt hai ki instance unhealthy hai.
 
Tum ```terraform plan``` run karoge to terraform bolega, config same hai, mujhe kuch change nhi karna.

Par tum chahte ho:
- Ki ye resource fir se re-create ho.

TO yaha use hota hai ```terraform taint```.

<br>

**Terraform Taint Actual Me Kya Karta Hai?**:

Terraform taint:
- Resource ko state file me "tainted = true" mark karta hai.
- Next ```terraform apply``` me:
  - Pehle destroy.
  - Fir recreate.

<br>

**Command**:
```
terraform taint aws_instance.my_ec2
```
Fir
```
terraform apply
```

<br>

**Kab Use Karte Hain Terraform Taint?**

Jab:
- Resource corrupted ho.
- Manual change ho gaya ho.
- Tum clean rebuild chahte ho.
- Debugging ke time.

<br>

**Deep Interview Insight (Advanced Point)**

Terraform v0.15+ me:
- ```terraform taint``` deprecated hone wala hai

Recommended approach:
```
terraform apply -replace="aws_instance.my_ec2"
```
Ye safe & controlled replacement hota hai.
