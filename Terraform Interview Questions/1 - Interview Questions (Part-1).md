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


