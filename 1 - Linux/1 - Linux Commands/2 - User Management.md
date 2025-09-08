# User Management Commands

<img src="https://drive.google.com/uc?export=view&id=13OQeUF24mgxJ1F6E3kxe8haMZl0ozLuS" width="650" height="420">

<br>

Linux ek multi-user operating system hai, jisme ek hi machine par multiple users alag-alag kaam karte hain. Security, permissions aur resource allocation manage karne ke liye user aur group management bahut zaroori hai.

<br>

### cat /etc/passwd

Ye command users ki list dekhne ke liye hoti hai

Example:
```
cat /etc/passwd
```

<br>

### useradd:

Ye command ek new user banane ke liye hoti hai. Agar ek user create karo to ye command user ka home directory, shell, aur group assign karti hai.

Syntax:
```
sudo useradd [options] username
```

Examples:

- Ek simple user banane ke liye:
```
sudo useradd testusr
```

<br>

### cat /etc/group

Ye command list of group dekhne ke liye hoti hai.

Example:
```
cat /etc/group
```

<br>

### groupadd

Ye command ek new group create karti hai. Groups ek tarika hote hain multiple users ko ek hi set of permissions dene ka.

Syntax:
```
sudo groupadd groupname
```

Examples:

- Ek group create karna:
```
sudo groupadd developers
```

<br>

### adduser

Ye command interectively user ko add karti hai. ```adduser``` ek interactive script hai jo useradd ko internally use karta hai. Ye tumse questions poochta hai jaise: full name, password, home directory, shell, etc.

Syntax:
```
sudo adduser username
```

Examples:

- Ek user create karna aur details fill karna:
```
sudo adduser john
```
(Ye tumse password, full name, phone number etc. puchhega)

<br>

### passwd

Ye command ek user ka password set/change karne ke liye use hoti hai.

Syntax:
```
passwd username
```

Examples:

- Current user ka password change karna:
```
passwd
```

- Specific user ka password change karna:
```
sudo passwd testusr
```

<br>

### usermod

Ye command ek existing user account modify karne ke liye hoti hai. Isse user ko groups mein add, shell change, home directory change, etc. kar sakte ho.

Syntax:
```
sudo usermod [options] username
```

Examples:
- User ko ek group mein add karna:
```
sudo usermod -aG sudo testusr
```
- sudo=group
- testuser=user
- Iska matlab hu, testuser ko sudo ke group main add kar rhe hain.

<br>

### deluser

Ye command ek user ko delete karne ke liye hoti hai. Isme user ka home directory default delete nahi hota.

Syntax:
```
sudo deluser username
```

Examples:
- User delete karna:
```
sudo deluser testusr
```

- User aur uska home directory delete karna:
```
sudo deluser --remove-home testusr
```

<br>

### groupdel

Ye command ek group ko delete karne ke liye hoti hai.

Syntax:
```
sudo groupdel groupname
```

Examples:
- Ek group delete karna:
```
sudo groupdel developers
```
- Agar koi user abhi bhi us group ka member hai to group delete nahi hoga.

<br>

### groups

Ye command ek user kitne groups main add hai usko check karti hai.

Syntax:
```
groups username
```

Examples:
- Current user ke groups dekhna:
```
groups
```

- Specific user ke groups dekhna:
```
groups testusr
```

<br>

### usermod -aG

Ye ek special option hai jo ek user ko kisi additional group mein add karne ke liye hota hai (bina uske existing groups ko hataye).

Syntax:
```
sudo usermod -aG groupname username
```

Examples:
- User ko sudo group mein add karna:
```
sudo usermod -aG sudo testusr
```

- User ko multiple groups mein add karna:
```
sudo usermod -aG developers,docker testusr
```

<br>
<br>

## Real-World Example Scenario

Maan lo tumhe ek project ke liye naye developers ko onboard karna hai:

1 - Ek group create karo sab developers ke liye:
```
sudo groupadd developers
```

2 - Ek user add karo ek developer ke liye:
```
sudo adduser dev1
```

3 - User ko developers group mein add karo:
```
sudo usermod -aG developers dev1

4 - Password set/change karo:
```
sudo passwd dev1
```

5 - Jab project complete ho jaye, user ko remove karna hai:
```
sudo userdel -r dev1
sudo groupdel developers
```
```
