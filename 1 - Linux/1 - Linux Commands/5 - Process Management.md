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

- **Runtime ke time** → Jab linux already running mein hai aur jab koi program run karna ho to tab ek process ```fork()``` karke ek naya process ya system call banata hai aur uske baad ```exec()``` call karke ek naya program us process me load kar deta hai. Iska matlab hai ki jab linux already running mein hai aur humko ek program jaise ```ls``` run karna hai to ek ```fork()``` process hoti hai vo run hoti hai aur parent process ```systemd``` se ek child process create kar deti hai, fir ek ```exec()``` process humare program ko child process mein load karke program ```ls``` run kar deti hai.

<br>

**Process Creation Steps in Linux**:

<br>

**Step 1: fork() – Copy of Parent**:
- Jab ek running process ko naya process banana hota hai, wo fork() system call use karta hai. Jaise, ```systemd()``` process ```fork()``` call karega.
- Ye ek child process banata hai jo parent ki copy hoti hai.
- Child process ke paas:
  - Naya PID (Process ID) hota hai.
  - Parent process ID (PPID) assigned hoti hai.
  - Resources (memory, file descriptors) initially parent ke clone hote hain.

Yaha Copy-On-Write (COW) ka concept aata hai – Matlab initially parent aur child dono ek hi memory ko share karte hain, lekin jaise hi koi write operation hota hai, alag copy allocate hoti hai.

**Step 2: exec() – Replace with New Program**:
- Fork ke baad, child process me aksar ```exec()``` family ka system call use kiya jata hai.
- ```exec()``` child process ke andar jo code chal raha tha use replace karke ek naya program load kar deta hai.

Example:

Agar tum terminal me ```ls -l``` likhte ho:
- Shell ek child process banata hai ```fork()``` se.
- Fir us child ke andar ```exec()``` se ls program load hota hai.

Isse child ek completely naya program run karta hai, lekin parent (yani shell) apna kaam jaari rakhta hai.

**Step 3: Parent-Child Synchronization (wait())**:
- Jab parent ek child process banata hai, uske baad parent ko decide karna hota hai ki:
  - Wait kare child ke khatam hone ka (```wait()``` system call se).
  - Ya fir parallel me apna kaam continue kare.
 
Isliye Linux multi-tasking system hai – ek hi time me multiple processes run kar sakte hain.

**Step 4: Process Termination (exit)**:
- Jab process ka program complete ho jata hai to wo ```exit()``` system call call karta hai.
- OS us process ka status parent ko send karta hai.
- Parent ```wait()``` karke us exit status ko read kar sakta hai.
- Agar parent ne wait nahi kiya to wo process zombie process ban jata hai (terminated process jiska exit status abhi parent ne read nahi kiya).

<br>

### Example Flow – Shell ke andar command run karna

Socho tum terminal me likhte ho:
```
ls -l
```
- Tumhari shell ek process hai.
- Shell ```fork()``` karegi → ek naya child process banega.
- Child process ```exec()``` call karega → ```ls``` program usme load hoga.
- ```ls -l``` ka output terminal par print hoga.
- Jab ```ls``` khatam ho jaega → child process ```exit()``` karega.
- Shell parent process hai → wo ```wait()``` karke child ka status lega aur fir wapas tumse agla command input lega.

### Diagram (Process Tree Example)

```
init (PID 1)
  |
  └── bash (PID 1000)
        |
        ├── fork() → child bash (PID 1001)
        |        |
        |        └── exec() → "ls -l" program (PID 1001)
        |
        └── parent bash waits for child
```

<br>
<br>

### Example "tumne abhi bataya ki init ya systemd se child process run hota hai lekin uper example main shell run kar rha hai?":

**Step 1: Sabse pehla process** – ```init``` / ```systemd```:
- Jab tumhara Linux machine boot hota hai → kernel load hota hai.
- Kernel apne initialization ke baad sabse pehle ek process create karta hai, jiska naam historically ```init``` (PID 1) hota tha.
- Aaj ke modern Linux distros me ye role ```systemd``` ne le liya hai.
- Matlab root parent of all processes = systemd.

```systemd``` ka kaam hai:
- System services start karna (jaise network, login, etc.).
- Background daemons chalana.
- Aur finally ek login shell start karna (jaise /bin/bash, /bin/zsh).

<br>

**Step 2: Shell kaise aata hai?**:
- Jab tum terminal open karte ho (ya system login hota hai), systemd ek shell process start karta hai.
- Example:
  - Tum login karte ho console me → ```systemd``` tumhare liye ```bash``` run karega.
  - Matlab shell khud bhi ek child process hai jo systemd ne banaya hai.
 
<br>

**Step 3: Shell ka role**:
- Ab shell chal rahi hai aur tum input dete ho, jaise ```ls -l```.
- Shell ek normal process hai (child of systemd).
- Jab tum command dete ho:
  - Shell ```fork()``` karke ek child process banati hai.
  - Child process ```exec()``` karke ls program load karta hai.
  - Parent shell wait karta hai jab tak child (ls) khatam na ho jaye.

**Process Tree**:
```
systemd (PID 1)          ← Sabse pehla process
   ├── NetworkManager     ← system service
   ├── gdm / getty        ← login screen / console login
   │     └── bash (PID 1000)   ← tumhara shell
   │            └── fork() → child bash (PID 1001)
   │                         └── exec() → ls -l (PID 1001)
   │
   └── aur bhi services (cron, sshd, etc.)
```

<br>
<br>


## Commands

<br>

### 1 - ps – Process Status

```ps``` ka matlab hai process status. Ye command tumhare system par current mein chal rahe processes ki information dikhati hai. Matlab tum dekh sakte ho kaun kaunsa process chal raha hai, uska PID kya hai, kaunse user ke under hai, kitni memory use kar raha hai, etc.

Jab tum sirf ```ps``` command likhte ho to ye sirf current terminal mein chal rhi process ko dikhayegi.

Syntax:
```
ps [options]
```

Example:
- Display running process in your current terminal:
```
ps
```
Output:
```
    PID TTY          TIME CMD
   2545 pts/4    00:00:00 bash
   3555 pts/4    00:00:00 ps
```
```
  - PID: Process Id
  - TTY: Terminal associated with the process.
  - Time: The amount of CPU time a process has consumed. It means how long a process is being running.
  - CMD: The command that was used to start the process.
```

- Display all processes running in your system:
```
ps -A
```

- Display all processes in full format It display all process in long format:
```
ps -AF
```
Output:
```
UID          PID    PPID  C    SZ   RSS PSR STIME TTY          TIME CMD
root           1       0  0  5466 13140   6 15:01 ?        00:00:04 /sbin/init
root           2       1  0   654  1440  11 15:01 ?        00:00:00 /init
root           6       2  0   654   132   5 15:01 ?        00:00:00 plan9 --control-socket 6 --log-level 4 --server-fd 7 --pipe-fd 9
root          60       1  0 20832 24084   4 15:01 ?        00:00:05 /usr/lib/systemd/systemd-journald
root         103       1  0  5995  6060  10 15:01 ?        00:00:03 /usr/lib/systemd/systemd-udevd
```
```
  - UID: User Id. It identifies which user is running the process.
  - PID: Process Id. A unique identifier for the process.
  - PPID: Parent Process Id. This shows which process started the current process.
  - C: CPU Usage. This shows the percentage of cpu time used by the process.
  - STIME: Start time of process. Indicates when the process was started.
  - TTY: Terminal Type. Indicates the terminal from which the process was started.
  - CMD: The command that was used to start the process.
```

- Display process for all user in user-oriented format including which are not associated with your terminal:
```
ps aux
```
```
a: Display process for all users.
u: Display process in user-oriented format.
x: Display process that are not attached to your terminal. This includes background processes.
```

<br>

### systemctl - Systemd Service Manager

```systemctl``` ek Linux ka command-line tool hai jo Linux systemd process ke upar bana hai aur system services, processes, aur units ko manage karta hai. Isse tum system ki services ko start, stop, restart, enable, disable, mask, unmask aur status check kar sakte ho.

```systemctl``` ek tool hai jo systemd units (services, targets, sockets, timers, etc.) ko manage karta hai.

Iska kaam hai:
- Services ko manage karna (jaise ki ssh, apache, mysql).
- Units ko control karna (unit matlab service, socket, target, path, timer, etc.).
- System ke state ko control aur monitor karna.

Syntax:
```
systemctl [OPTIONS] COMMAND [NAME]
```
- ```COMMAND``` → kya karna hai (start, stop, enable, status, etc.).
- ```NAME``` → kis service ya unit pe command chalani hai (jaise ```nginx.service```, ```sshd.service```).

<br>

**Example: Common systemctl commands**:

- **Service ko start karna**:
```
systemctl start nginx
```
Ye command nginx service ko abhi turant start karega.

- **Service ko stop karna**:
```
systemctl stop nginx
```
Running service ko band (stop) karne ke liye. 

e.g., Web server me kuch misconfiguration hai aur tum temporarily nginx ko rokna chahte ho.

- **Service ko restart karna**:
```
systemctl restart nginx
```
Service ko ek baar band karke phir se start karne ke liye.

e.g., Jab tum config file (```/etc/nginx/nginx.conf```) me change karte ho, tumhe service ko restart karna padta hai taaki naye configs load ho.

- **Service ko reload karna**:
```
systemctl reload nginx
```
Service ko bina band kiye config files reload karwane ke liye

e.g., Nginx me tumne ek virtual host add kiya hai aur config reload karna hai bina downtime ke.

- **Service ka status dekhna**:
```
systemctl status nginx
```
Kisi bhi service ka current status aur details dekhne ke liye yeh command use hoti hai.

Output me milega:
  - Active/inactive (running ya stopped).
  - PID aur processes list.
  - Logs (last few journalctl entries).

- **Service ko enable karna (boot ke time auto start)**:
```
systemctl enable nginx
```
Agar tum chahte ho ki nginx server system restart hone par khud se restart ho jaye to nginx ko enable karna hoga.

e.g., Agar tumhe chahiye ki machine restart hone par nginx automatically start ho jaye → use enable.

- **Service ko disable karna (boot ke time auto start na ho)**:
```
systemctl disable nginx
```
Is command se reboot ke baad service auto start nahi hogi.

- **Service ko mask (matlab disable) karna**:
```
systemctl mask nginx
```
Ye command service ko completely disable kar deti hai.

e.g., Isse service completely disable ho jati hai (koi start nahi kar sakta manually bhi).

- **Service ko unmask (matlab enable) karna**:
```
systemctl unmask nginx
```
Isse service wapas usable ho jati hai.
e.g., Agar koi service galti se system ke stability kharab kar rahi hai aur tum chahte ho ki koi user galti se use start na kare.

- **System ko reboot/shutdown karna**:
```
systemctl reboot
systemctl poweroff
systemctl halt
```
  - ```reboot``` → system restart hoga.
  - ```poweroff``` → machine shutdown hogi.
  - ```halt``` → CPU ko stop kar deta hai but power off nahi karta.

- **All active services list karna**:
```
systemctl list-units --type=service
```
Ye command tumko active service ki list dikhayegi.

- **All active-inactive services list karna**:
```
systemctl list-unit-files --type=service
```
Ye command tumko sabhi active, inactive, masked, unmasked service dikhayegi.

