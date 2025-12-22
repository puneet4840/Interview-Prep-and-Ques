# Terraform Interview Questions Part-2

### Question:

Que-1: What is the difference between ```count``` and ```for_each```? Give a real-world use case.


<br>
<br>

## Que-1: What is the difference between ```count``` and ```for_each```? Give a real-world use case.

<br>

### Sabse Pehle Basic Idea Samjho

Terraform me kabhi-kabhi hume same type ka resource multiple times banana hota hai.

Example:
- 3 EC2 VMs.
- 5 Azure storage accounts.
- Multiple subnets.
- Multiple NICs.

To ye same type ke multiple resources ko create karne ke liye loop ka use hota hai.

Yahin pe Terraform hume two looping mechanisms deta hai:
- ```count```.
- ```for_each```.

Inko terraform mein meta arguments bhi bola jata hai.

<br>
<br>

### count

```count``` terraform mein ek meta-argument hai, jo ek tarah se terraform mein loop ki tarah use kiya jata hai.

```count``` tab use hota hai jab:
- Humko same type ke multiple resources banane ho.
- Kitne resources banane hain ye pta ho.

Matlab jaise humko 5 resource group banane hain jo same type ke hain, to hum ```count``` se 5 resource group create kar sakte hain.

```count``` number pe based hota hai matlab number vise resources banata hai, jaise agar 3 resource groups banane hain to un resource groups ka naame hoga:
- ```rg-0```.
- ```rg-1```.
- ```rg-2```.

<br>

**Basic Syntax**:

```
resource "azurerm_virtual_machine" "vm" {
  count = 3
  name  = "vm-${count.index}"
}
```

Iska matlab:
- Terraform 3 VMs banayega.
- Names honge:
  - vm-0.
  - vm-1.
  - vm-2.


```count.index``` Kya Hai?
- ```count.index``` ek number hota hai.
- 0 se start hota hai.
- Har resource ko ek index deta hai.

<br>

**Real-World Use Case of count**

Scenario:

Tumhe 3 identical test VMs chahiye:
- Same size
- Same image
- Same network
- Same tags

Bas quantity change ho sakti hai.

```
variable "vm_count" {
  default = 3
}

resource "azurerm_linux_virtual_machine" "test_vm" {
  count = var.vm_count
  name  = "test-vm-${count.index}"
}
```

To ye 3 same vms bana dega bss unke naam mein 0, 1 aur 2 honge.

<br>

**count Ka Sabse Bada Problem**:

Count ka sabse bada problem hota hai Index Problem jisko Index Shifting kehte hain.

Socho tumko 3 vms banane hain:
- Tumne ek list banai jisme vm ke environments likhe hue hain, jaise ```["dev","qa","prod"]```:
- Ab tumko is list ke elements ko lena hai count ke through indexing se.
- Tumne ```count``` list ke length ke barabar le liya, jisse list ko traverse kiya ja sake.
- Tumne vm bana diya.

Terraform banata hai:
```
vm-dev, vm-qa, vm-prod
```

Ab tumne list mein se ek element ```"prod"``` delete kar diya aur terraform plan/apply chala diya, to yaha pe terraform prod vm ko delete kar dega.

Suppose list mein abhi bhi teeno elements hi hain ```["dev","qa","prod"]```, tumne 2nd wala ```"qa"``` element delete kar diya aur terraform pla/apply chala diya to, yaha pe problem hoti hai index shifting ki.

Terraform ki state file mein pehle 2nd position par qa element tha, lekin tumne qa ko delete kar diya aur ab 2nd position par prod aa gya, isse hoga ye ki terraform ko prod ki nayi position mili, to pehle terraform 3rd position pe jo prod vm tha usko cloud mein delete karega aur prod ko 2nd position par dekhkar, fir se prod vm ko 2nd position ke hisab se create karega, 

To yaha pe agar element shift hote hain to delete hoke fir se re-create hoga. Yehi problem hain ```count``` saath.

<br>
<br>

### for_each

```for_each``` ek key-based loop hai.
