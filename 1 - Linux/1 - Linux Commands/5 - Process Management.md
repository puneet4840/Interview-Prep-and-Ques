# Process Management

<img src="https://drive.google.com/uc?export=view&id=1GjwR2D_h0kBch3bBB4LXWkXBsdsJkaGr" width="450" height="720">

<br>

### Process kya hota hai?

Process ek running program hota hai. Jab tum Linux mein koi program chalate ho (jaise ```ls```, ```vim```, ```nginx```), toh wo disk se uthkar memory mein load hota hai aur running instance ban jaata hai. Us running instance ko hi Process kehte hain.

Har process ka apna:
- PID (Process ID) hota hai.
- Memory space hoti hai.
- CPU time hota hai.
- File descriptors hotte hain.

Linux ek multi-user, multitasking OS hai. Matlab ek time par multiple processes parallel chal sakte hain aur OS inko manage karta hai.

<br>

### Process creation in Linux

Linux mein har process ek aur process se banta hai matlab linux mein ek parent process hota hai jo child process banata hai, jab bhi hum linux mein koi command run karte hain jaise ```ls``` ye sabhi child process hote hain.

Linux mein process do tareeke se create hoti hai:
- **System boot ke time** → Sabse pehla process ```init``` (ya modern systems me ```systemd```) create hota hai. Ye har baaki process ka ancestor hota hai. Yahan system boot ka matlab jab linux fresh start hota hai ya restart hota hai. To jab linux start hota hai to sabse pehla process ```init``` run hota hai jiski PID 1 hoti hai ya modern systems mein ```systemd``` process run hota hai, jiski PID ```1``` hoti hai, Inke baad jitne bhi process run hote hain vo inhi process se create hote hain aur inke child process hote hain.

- **Runtime ke time** → Jab linux already running mein hai aur jab koi program run karna ho to tab ek process ```fork()`` karke ek naya process banata hai aur uske baad ```exec()``` call karke ek naya program us process me load kar deta hai. Iska matlab hai ki jab linux already running mein hai aur humko ek program jaise ```ls``` run karna hai to ek ```fork()``` process hoti hai vo run hoti hai aur parent process ```systemd``` se ek child process create kar deti hai, fir ek ```exec()``` process humare program ko child process mein load karke program ```ls``` run kar deti hai.

