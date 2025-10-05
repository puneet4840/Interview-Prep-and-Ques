# Disk Management

<img src="https://drive.google.com/uc?export=view&id=1pmjPw6CMvVa_zjf9_k_1XtLLbyJKDSpj" width="520" height="190">
<img src="https://drive.google.com/uc?export=view&id=1LELg75P8KsUIT-vO-hPJgqCtSyIkd37B" width="520" height="190">

<br>

### Disk ka Concept Linux me

Linux system me Disk ka matlab hota hai woh physical ya virtual storage device jisme tumhara OS, applications aur data store hota hai. Ye HDD (Hard Disk Drive), SSD (Solid State Drive) ya Virtual Disk (jaise Azure/AWS/VMware ke VMs me) ho sakti hai.

Linux me disk ko **block device** bola jata hai kyunki ye data ko blocks ke form me read/write karta hai.

Ye wo jagah hai jisme tumhara Linux system aur data store hota hai. Jaise tumhare laptop/PC me C drive, D drive hote hain, waise hi Linux me disk hoti hai.

Linux mein disk ```/dev/``` directory me ek special file ke roop me dikhai deti hai.

Example:
```
/dev/sda
/dev/sdb
```

<br>

**Partition kya hai?**:

Disk ko chhote-chhote parts me divide kar dete hain. Har part ko partition bolte hain.

Disk ko multiple parts mein divide karne ko partitioning kehte hain. Har partition alag-alag use ke liye hota hai, jaise root (/), home (/home), ya swap.

Example:
```
/dev/sda1  → pehla partition
/dev/sda2  → dusra partition
```

<br>

**File system kya hai?**:

Partition me data tab tak use nahi kar sakte jab tak usme ek file system na ho (ext4, xfs, ntfs, etc.).

File system ek tarika hai data ko organize karne ka. File system data ko structure deta hai, jaise folders, files, permissions.

Common File Systems in Linux:
- ```ext4```: Default for Ubuntu, reliable, journaling (crash se data safe).
- ```XFS```: High performance for large files.
- ```Btrfs```: Advanced, snapshots, RAID support.
- ```NTFS/FAT32```: Windows compatibility ke liye.

<br>

**Mount point kya hai?**:

Partition ko ek folder me attach karna. Us folder ko mount point bolte hain.

Example:
- ```/``` → root filesystem (main system).
- ```/home``` → users ka data.
- ```/var/log``` → logs.

<br>

**Disk ka logical structure**:

Ek disk ko linux is tarah organize karta hai:
```
[ Disk (/dev/sda) ]
   ├── Partition 1 (/dev/sda1)
   │       └── File System (ext4, xfs, btrfs…)
   │             └── Mount Point (/)
   ├── Partition 2 (/dev/sda2)
   │       └── File System (ext4)
   │             └── Mount Point (/home)
   └── Partition 3 (/dev/sda3)
           └── Swap area
```

<br>
<br>

## Commands for Disk Management

<br>

### df

```df``` ka full form hai **disk filesystem** (isko **disk free** bhi bolte hain) hai. 

Ye command tumhe batata hai ki system ke mounted filesystems par kitni storage total hai, kitni use ho rahi hai, aur kitni available hai.

DevOps/Cloud context mein, jab disk full ho jati hai to services fail kar sakte hain — ```df``` sabse pehla tool hai jo check karte ho.

<br>

**Syntax**:
```
df [options] [file-or-mountpoint]
```

<br>

**Commonly used options**:

Jab ```df``` command likhte hain to ye KB main data dikhata hai.

- ```-h```: Human readable format main data dikhata hai. sizes ko KB/MB/GB me dikhata hai (e.g. ```4.0G```).
- ```-H``` : human-readable using powers of 1000 (instead of 1024).
- ```-T``` : filesystem type bhi show karo (ext4, xfs, tmpfs, nfs...).

<br>

**Examples**:

**1 - KB main disk ki information dekhna**:
```
df
```
Output:
```
Filesystem      1K-blocks      Used Available Use% Mounted on
none              3945048         0   3945048   0% /usr/lib/modules/5.15.167.4-microsoft-standard-WSL2
none              3945048         4   3945044   1% /mnt/wsl
drivers         497775612 217709664 280065948  44% /usr/lib/wsl/drivers
/dev/sdc       1055762868  10751460 991307936   2% /
```

**2 - Human readable formate MB/GB/TB main disk ki information dekhna**:
```
df -h
```
Output:
```
Filesystem      Size  Used Avail Use% Mounted on
none            3.8G     0  3.8G   0% /usr/lib/modules/5.15.167.4-microsoft-standard-WSL2
none            3.8G  4.0K  3.8G   1% /mnt/wsl
drivers         475G  208G  268G  44% /usr/lib/wsl/drivers
/dev/sdc       1007G   11G  946G   2% /
none            3.8G  188K  3.8G   1% /mnt/wslg
```

Explanation:
- ```Filesystem```: Filesystem/Disk ka naam.
- ```Size```: filesystem ki total size.
- ```Used```: kitna space already used hai.
- ```Avail```: kitna space non-root users ke liye available hai.
- ```Use%```: (Used / Size) * 100 ka rounded percent. Kitne percent used hai.
- ```Mounted on``` — mount point, jis directory main disk mount hai.

**3 - Specific mountpoint ya file ka report**:
```
df -h /var/log
```

<br>
<br>

### du 

```du``` ka full form hai disk usage.

```df``` batata hai filesystem-level summary (kitna total, used, available space hai).

```du``` batata hai directory/file-level usage (kaun si file/directory kitna disk space consume kar rahi hai).

Matlab:
- Agar tumhe overall storage usage dekhna hai → ```df```.
- Agar tumhe dekhna hai “kis folder ne disk bhar rakha hai?” → ```du```.

<br>

**Syntax**:
```
du [options] [file/directory...]
```

Default behavior:
- Agar bina option ke chalaya jaaye: ```du``` har subdirectory ka size (in blocks) print karega.
- Default unit = 1 KB blocks (system dependent).

<br>

**Commonly used options**:
- ```-h``` → human readable (KB/MB/GB me size batayega).
- ```-s``` → summarize (sirf total size, subdirectories ka detail nahi).
- ```-a``` → files ke sizes bhi show karo (default = sirf directories ke cumulative size).

<br>

**Examples**:

**1 - Simple usage**:
```
du /var/log
```
Ye ```/var/log``` ke andar sab directories ka size (KB blocks) dikhayega.

Output:
```
du: cannot read directory '/var/log/private': Permission denied
4       /var/log/private
104     /var/log/apt
4       /var/log/landscape
108     /var/log/grafana
```

**2 - Human readable**:
```
du -h /var/log
```
Ye human readable matlab size ko MB/GB main dikhayega.

Output:
```
du: cannot read directory '/var/log/private': Permission denied
4.0K    /var/log/private
104K    /var/log/apt
4.0K    /var/log/landscape
108K    /var/log/grafana
4.0K    /var/log/dist-upgrade
376M    /var/log/journal/c66466fa1fff48e7898964223bf51bc4
376M    /var/log/journal
```

**3 - Summarize only total**:
```
du -sh /var/log
```
Ye ```/var/log``` ka total size batayega.

Output:
```
519M    /var/log
```

**4 - Show all files size also**:
```
du -ah /var/log | sort -h | tail -n 10
```
Ye sabse bade 10 files/folders dikhata hai jo ```/var/log``` me space kha rahe hain.

<br>
<br>

### fdisk

```fdisk``` ka full form hai **Fixed Disk Editor**.

Ye command disk ke partition ko manage karne ke kaam aati hai, jaise disks ko partition dekhne, create karne, delete karne, aur modify karne ki facility deta hai.

Basically: agar tumhe nayi hard disk mount/use karni hai, toh pehle usme partition create karna padta hai → ```fdisk``` kaam aata hai.

<br>

**Syntax**:
```
fdisk [options] <disk>
```

<br>

**Real-Life Use Case (DevOps/Cloud Engineer ke liye)**:

- Agar tumne vm mein ek new EBS volume (AWS) ya Persistent Disk (GCP) attach kiya hai, toh pehle ```lsblk``` ya ```fdisk -l``` karke check karoge ki naya disk kaun sa device hai.
- Fir ```fdisk /dev/xvdf``` chalake partition banaoge.
- ```mkfs.ext4``` se format karoge.
- `Mount karke ```/etc/fstab``` mein entry dal doge.

<br>

**fdisk Interactive Mode ke Options**:

Jab tum ```fdisk /dev/sda``` chalaoge, tumhe ek prompt milega:
```
Command (m for help):
```

Common commands:
- ```m``` → help menu dikhata hai (commands list).
- ```p``` → current partition table print karo.
- ```n``` → naya partition banao.
- ```d``` → partition delete karo.
- ```t``` → partition ka type change karo (Linux, swap, EFI, etc.).
- ```a``` → bootable flag toggle karo.
- ```w``` → changes save karo aur exit.
- ```q``` → bina save kiye quit.

<br>

**Partition Creation ka Example**:

Step 1: Disk check karo:
```
fdisk -l
```
Maan lo ek nayi disk hai ```/dev/sdb``` (100GB).

Step 2: Start fdisk:
```
fdisk /dev/sdb
```

Step 3: New partition banao:
- ```n``` dabao → new partition create karne ke liye.
- Pucha jaayega: primary (p) ya extended (e) → choose p (primary).
- Partition number: 1
- First sector: default enter kar do.
- Last sector: specify karo (ya size likh do, e.g. +20G).

Step 4: Partition ka type set karo (optional):
- ```t``` → type choose karo (e.g. Linux = 83, Swap = 82).

Step 5: Partition table dekhne ke liye:
- ```p```.

Step 6: Changes save karo:
- ```w``` → write to disk and exit.

Example Output:
```
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-20971519, default 2048): 
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): +20G

Created a new partition 1 of type 'Linux' and of size 20 GiB.
```

<br>

**Next Steps after Partitioning**:

```fdisk``` sirf partition create karta hai, lekin usme filesystem nahi banata.

Partition banane ke baad:
- Filesystem create karo:
```
mkfs.ext4 /dev/sdb1
```

- Mount point banao:
```
mkdir /mnt/data
```

- Mount partition:
```
mount /dev/sdb1 /mnt/data
```

- Permanent mount (fstab mein entry):

/etc/fstab mein line add karo:
```
/dev/sdb1   /mnt/data   ext4   defaults   0   0
```
