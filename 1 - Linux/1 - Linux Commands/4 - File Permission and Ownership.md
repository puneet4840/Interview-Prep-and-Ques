# File Permissiona and Ownership

<img src="https://drive.google.com/uc?export=view&id=1woMVejns0iEuLQEuRLjW4S-P_HK239Cu" width="750" height="420">

<br>

### Permission and Ownership in Linux

Linux ek multi-user OS hai. Iska matlab hai ki ek hi system par ek saath alag-alag log login karke kaam kar sakte hain.

Isliye har file/directory par ye define karna zaroori hai ki:
- Kaun file ko access kar sakta hai (ownership).
- Kaise access kar sakta hai (permissions: read/write/execute).

Isliye har file/directory par 3 cheezein define hoti hain:
- **Owner (User)** – Kaun iska malik hai.
- **Group** – Kaunse user group ko access hai.
- **Permissions (r, w, x)** – Kaun kya kar sakta hai (read/write/execute).

Ye sab mila ke tum decide karte ho ki ek file/directory kaun access karega aur kaise karega. Ye hi Linux ka core security mechanism hai.

**Ownership – User aur Group**:

Linux mein har file/directory par do ownership hote hain:
- User (Owner): File ka asli malik. Default me ye woh user hota hai jisne file banai hai.
- Group: Ek group ek saath me kai users ka collection hota hai. Group ke members ko ek hi set of permissions diye ja sakte hain.

Example:
```
ls -l file.txt
-rw-r----- 1 puneet devteam 2048 Sep 29 12:00 file.txt
```

Yahan:
- ```puneet``` = file ka owner hai, matlab ```puneet``` ne ye file banai hai.
- ```devteam``` = file ka group hai.
- ```-rw-r-----``` = permissions hain.

Group ka concept important isliye hai kyunki agar 5 developers ek project par kaam kar rahe hain to un sabko ek group me daal do (jaise ```devteam```). Phir file ka group ```devteam``` set karo. Ab sab members ko ek saath permissions mil jayengi.

**Permissions: r, w, x ka matlab**:

Linux har file/directory ke liye 3 set of permissions rakhta hai:

| Set            | Kiske liye hota hai                              |
| -------------- | ------------------------------------------------ |
| **User (u)**   | File ke owner ke liye                            |
| **Group (g)**  | File ke group members ke liye                    |
| **Others (o)** | Baaki sab users (jo owner ya group me nahi hain) |

Har set me teen types of permissions hoti hain:

| Symbol | Number | Matlab                                                           |
| ------ | ------ | ---------------------------------------------------------------- |
| **r**  | 4      | Read (file ka content dekh sakte ho)                             |
| **w**  | 2      | Write (file ko modify/delete kar sakte ho)                       |
| **x**  | 1      | Execute (file ko run kar sakte ho / directory open kar sakte ho) |

Agar kisi file ko koi permission nahi deni hai to uski permission hum 0 de sakte hain.

| **-**  | 0      | No Permission |

Inko add karke numeric form me likhte hain.

Example:
- rwx = 4+2+1 = 7
- rw- = 4+2+0 = 6
- r-x = 4+0+1 = 5

**Files aur Directories par Permission ka Difference**:

| Permission | File par                                 | Directory par                             |
| ---------- | ---------------------------------------- | ----------------------------------------- |
| **r**      | File ka content read karna               | Directory ke contents list karna (ls)     |
| **w**      | File me changes karna                    | Directory me new file create/delete karna |
| **x**      | File ko execute/run karna (jaise script) | Directory me `cd` karke enter karna       |

Example:
- Agar directory par ```x``` nahi hai to tum ```cd``` karke usme nahi ja paoge.
- Directory par write (w) nahi hai → usmein new file nahi bana sakte.

**Numeric (Octal) Notation Detail**:

Linux mein permissions ko numeric values se bhi set karte hain:

| Value | Permission |        Matlab          |
| ----- | ---------- | --------------------   |
| 7     | rwx        |     Full Control       |
| 6     | rw-        |     Read + Write       |
| 5     | r-x        |     Read + Execute     |
| 4     | r--        |     Read               |
| 3     | -wx        |     Write + Execute    |
| 2     | -w-        |     Write              |
| 1     | --x        |     Execute            |
| 0     | ---        |     No Permission      |

Example:
```
chmod 755 file.txt
```
Matlab:
- Owner = 7 (rwx).
- Group = 5 (r-x).
- Others = 5 (r-x)

**Commands**:

<br>

**chmod** (change mode): Ye commnad file aur directory ki permission change karne ke liye use hoti hai.

Syntax:
```
chmod <permission> file_name
```

Examples:
- Symbolic way:
```
chmod u+x file.sh       # Owner ko execute permission do
chmod g-w file.sh       # Group se write permission hatao
chmod o-r file.sh       # Others se read permission hatao
chmod a+r file.sh       # Sabko read permission do
```

- Numeric Way:
```
chmod 644 file.txt  # Owner=rw-, Group=r--, Others=r--
chmod 700 script.sh # Owner=rwx, Group=---, Others=---
```

Examples:
- Script ko owner ke liye read+write+execute aur baaki ke liye sirf read+execute dena:
```
chmod 755 script.sh
```
(User = rwx, Group = r-x, Others = r-x)

- File ko sirf owner ke liye readable aur writable banana:
```
chmod 600 secret.txt
```
(User = rw-, Group = ---, Others = ---)

- Symbolic method use karke execute permission dena:
```
chmod u+x script.sh
```
(u = user/owner, g = group, o = others, a = all)

- Directory aur subdirectories ke liye recursively permission dena:
```
chmod -R 755 /var/www/html
```

Real-World Use Case:
- Web servers (Apache/Nginx) mein HTML files ko 644 (rw-r--r--) aur directories ko 755 (rwxr-xr-x) set karte hain

<br>
<br>

**chown** (change owner/group): Ye command file aur directory ke user aur group change karne ke liye hoti hai.

Syntax:
```
chown [new_owner]:[new_group] filename
```

Example:
```
sudo chown puneet file.txt        # Owner ko puneet banao
sudo chown puneet:devteam file.txt  # Owner & group dono change karo
sudo chown -R www-data:www-data /var/www  # Recursively change ownership
```

- File ka owner change karna:
```
sudo chown testusr file.txt
```
- File ka owner aur group dono change karna:
```
sudo chown testusr:testgrp file.txt
```
- Directory aur subfiles ka ownership recursively change karna:
```
sudo chown -R www-data:www-data /var/www/html
```

Real-World Use Case:
- Web applications mein files ko web server user (e.g., www-data) ke ownership mein rakhna hota hai taaki server unhe access kar sake.

<br>
<br>

**chgrp** (Change Group Ownership): Ye command specifically file ya directory ke group ownership change karti hai.

Syntax:
```
chgrp [new_group] filename
```

Example:
- File ka group change karna:
```
sudo chgrp testgrp file.txt
```
- Directory ke andar ke sab files ka group change karna:
```
sudo chgrp -R devteam /project
```

**umask** (User Mask): umask ek default permission mask hota hai jo nayi files aur directories ke liye default permissions decide karta hai. Matlab jab bhi aap linux main koi nayi file ya directory create karenge uske liye default permission set ho jayegi jo hum umask ke through denge.

Default file permissions:
- File = ```666 (rw-rw-rw-)```.
- Directory = ```777 (rwxrwxrwx)```.

Umask in defaults se subtract hota hai. Aur jo bhi umask ki value aati hai vo permission ki default value ban jati hai, jaise without umask aap file create karoge to uski permissions 666 hogi, lekin 

Syntax:
```
umask value
```

Examples:
- Default umask check karna:
```
umask
```
Output:
```
0022
```
By default, umask 0022 hota hai, agar apko 022 karna hai to example follow karo.

- Umask ```022``` set karna:
```
umask 022
```

Files: ```666 - 022 = 644 (rw-r--r--)```.
Directories: ```777 - 022 = 755 (rwxr-xr-x)```.

- Umask ```027``` set karna:
```
umask 027
```

Files: 640 (rw-r-----).
Directories: 750 (rwxr-x---).

Real-World Use Case:
- Servers par sensitive data ke liye umask ```027``` set karte hain taaki files sirf owner aur group access kar saken, others ke liye deny ho.
- File ka owner change karna:
```
sudo chown testusr file.txt
```


