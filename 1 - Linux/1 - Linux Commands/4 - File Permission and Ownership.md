# File Permissiona and Ownership

<img src="https://drive.google.com/uc?export=view&id=1woMVejns0iEuLQEuRLjW4S-P_HK239Cu" width="750" height="420">

<br>

### chmod (Change Mode)

```chmod``` ka use file ya directory ke permissions (read, write, execute) ko set/modify karne ke liye hota hai. Linux mein permissions User (owner), Group, aur Others ke liye alag-alag define hote hain.

- Read (r) = 4
- Write (w) = 2
- Execute (x) = 1

Numeric value nikalne ke liye inhe add karte hain.

Example:
- ```7 = 4+2+1 = rwx```
- ```5 = 4+1 = r-x```
- ```6 = 4+2 = rw-```

Syntax:
```
chmod [permissions] filename
```

Examples:
- Script ko owner ke liye ```read+write+execute``` aur baaki ke liye sirf ```read+execute``` dena:
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
User ko execute permission dena.

(u = user/owner, g = group, o = others, a = all)

- Directory aur subdirectories ke liye recursively permission dena:
```
chmod -R 755 /var/www/html
```

Real-World use cae: Web servers (Apache/Nginx) mein HTML files ko 644 (rw-r--r--) aur directories ko 755 (rwxr-xr-x) set karte hain.


<br>

### chown (Change Owner)

```chown``` ka use ek file/directory ke owner ya group ownership change karne ke liye hota hai.

Syntax:
```
chown [new_owner]:[new_group] filename
```

Examples:
- File ka owner change karna:
```
sudo chown testusr file.txt
```


