# Terraform Interview Questions (Part-1)

### Questions:
- Que-1: What is the difference between terraform import and terraform taint?
- Que-2: How do you manage secrets in Terraform without hardcoding them?

<br>
<br>

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

<br>
<br>

## Que-2: How do you manage secrets in Terraform without hardcoding them?

Ab interviewr ye question puch sakta hai ki, Terraform mein secrets ko kaise manage kare?

To terraform mein secrets ko manage karne ke liye kayi methods hote hain, Jaise:
- Enviornment Variables ko use karna.
- Cloud Secret Manager ko use karo, jaise Azure Key Vault/ AWS Secret Manager.
- CI/CD pipeline mein secret use karne ke liye pipeline variables ka use karo.
- Terraform Sensitive Variables ka use kar sakte ho.

<br>

### Problem pehle samjho (Sabse Important)

**Problem kya hai?**

Terraform mein jab hum secret se deal karte hain to normally, ```.tf``` files mein secret ko use karke expose kar dete hain jaise password, client secret, database password, API key ko directly ```.tf``` file mein hard code kar dete hain.

Example (galat tareeka):
```
client_secret = "123abcd"
```

OR 

```
password = 12345
```

Lekin ye tarika bilkul galat hai, kyuki secrets ko hard karke use karne se secret reveal ho jata hai aur ye ek best practise nhi hai.

Ye kyun galat hai?
- Code Git me chala jayega.
- Team ke sab log dekh sakte hain.
- Hacker ko repo mil gaya → secret leak.
- Interview me ye bol diya → direct reject.

Isliye interviewer poochta hai: **“Without hardcoding secrets, kaise manage karte ho?”**.

<br>

### Iska Solution

**Golden Rule (Yaad rakhna)**:

Terraform ko secret ka VALUE nahi pata hona chahiye, Bas secret KAHAN se milega ye pata hona chahiye.

<br>

**Method 1: Environment Variable (Sabse simple)**:

Ye method hai ki humko secrets ko system ke Environment Variables mein rakhna hai na ki code mein.

Example:

Step-1: Terraform me variable banao:
```
variable "client_secret" {
  type      = string
  sensitive = true
}
```

Step-2: Secret ko system me set karo:

Local machine pe:
```
export TF_VAR_client_secret="my-secret"
```

**Note**: Terraform automatically ```TF_VAR_``` prefix wale environment variables ko read kar leta hai.

Step-3: Use karo:
```
client_secret = var.client_secret
```

Benefits:
- Secret code me nahi jata.
- Git me nahi jata.
- CI/CD pipelines me easily inject hota hai.

Limitation:
- Local machine me manually set karna padta hai.

<br>

**Method-2: CI/CD Pipeline Secrets (Azure DevOps/ Github Actions/Gitlab CI ya kisi bhi CI/CD tool me)**:

CI/CD pipelines mein secrets ko use karne ke liye hamesha pipeline variables ka use karo. Secret ko pipeline ke vairbale mein securely create kardo, fir terraform khud se hi enviornment se secret ko pick kar lega.


<br>

**Method-3: Cloud Secret Manager (Azure Key Vault/AWS Secret Manager)**:

Ye ek production ka best method hai ki secret ko cloud ke secret manager mein store karke use karo. Isme mein Azure Key Vault ka example dunga.

Terraform secret ko directly cloud ke secret manager se read karta hai.

Azure Key Vault ek cloud secret manager hai ko secret ko securly manage karne ke liye use hota hai.

Example:

Step-1: Secret Key Vault me rakho:

Suppose tumne azure mein ek key vault bana rakha hai aur uske ek secret rakh rakha hai:
- Name: sp-client-secret
- Value: xxxxx

Step-2: Terraform se secret read karo:

Terraform mein cloud se kisi bhi resource ki information fetch karne ke liye data_source block ka use kiya jata hai. Isliye cloud ke secret manager se secret ki information ko fetch karne ke liye yaha bhi data_source block ka use karenge.

```
data "azurerm_key_vault_secret" "sp" {
  name         = "sp-client-secret"
  key_vault_id = azurerm_key_vault.kv.id
}
```

Step-3: Use karo:
```
client_secret = data.azurerm_key_vault_secret.sp.value
```

Fayde:
- Secret rotate ho sakta hai
- Central secure storage
- Audit logs
- Enterprise standard


Interview Gold Line:
- For production, we use cloud-native secret managers like Azure Key Vault so Terraform never directly stores secrets in code.

<br>

**Method-4: Terraform Sensitive Variables**:

Ye ek aur method hota hai secrets ko manage karne ke liye, jisme terraform variable block mein variable define karte hain aur saath mein variable ki sestivity true kar dete hain. Jisse secret ki value logs mein hide rehti hai aur Plan/Apply ke output mein masked matlab hidden rehti hai.

Lekin, State file mein ye secret plain text mein aa sakta hai isliye, remote backend secure hona jaruri hai.

Example:
```
variable "db_password" {
  type      = string
  sensitive = true
}
```

<br>
<br>

### Ab ekdum simple summary yaad rakho

- Kabhi ```.tf``` file me password store nhi karna.
- Kabhi Git me secret commit/rakhna nhi hai.
- Kabhi secret Hardcoding nhi karni.

Agar ye follow karoge to secret secure rahega.

