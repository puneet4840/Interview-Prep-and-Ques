# File and Directory Management

<img src="https://drive.google.com/uc?export=view&id=196Xhm9YxKAbjysYLWgbdxMy-J5jVXO68" width="650" height="350">

<br>

### find

```find``` ek powerful command hai jo tumhe directory hierarchy ke andar recursively files aur directory search karne ki facility deti hai. Tum files aur directries ko name, type, size, modified time, owner aur permissions ke basis pe search kar sakte ho.

Syntax:
```
find [path] [options] [expression]
```

Example:
- Kisi directory ke andar sabhi .txt files dhundho:
```
find /home/test -name "*.txt"
```

- Specific file dhundho:
```
find / -name "names.txt"
```

- 100MB se badi files find karna:
```
find /home -size +100M
```

- Pichle 7 din ke andar modified files:
```
find /home -mtime -7
```

- Permission 777 wali files dhoondna:
```
find /home -type f -perm 0777
```

Real-World Use Case: Agar tumhe pata chalna hai ki kaun se logs file size bahut zyada bada ho gaya hai, tum find /var/log -size +500M use kar sakte ho.

<br>

### locate 

```locate``` ek super fast command hai jo ek prebuilt database (```mlocate.db```) use karke file names search karti hai. Ye ```find``` se zyada fast hai, lekin hamesha latest updates show nahi karti jab tak updatedb run na karo.

Syntax:
```
locate filename
```

Example:
- Ek file dhundho:
```
locate notes.txt
```

- Specific extension wali files dhundho:
```
locate "*.log"
```

- Agar database outdated hai, pehle update karo:
```
sudo updatedb
locate nginx.conf
```

Real-World Use Case: Agar tumhe jaldi se pata lagana hai ki ```nginx.conf``` file system ke kis path par hai, to ```locate nginx.conf``` best hai.

<br>

### tree

```tree``` ek command hai jo directories ko ek tree-like format mein display karti hai. Ye directory structure ko samajhne ke liye helpful hai.

Syntax:
```
tree [directory]
```

Examples:
- Current directory ka structure dekhna:
```
tree
```

- Specific directory ka structure dekhna:
```
tree /etc
```

- Only specific level tak display karna:
```
tree -L 2 /home
```

- Sirf directories show karna:
```
tree -d /var
```

Real-World Use Case: Tumhare paas ek web project hai aur tumhe uska folder structure samajhna hai (```tree project/```).

<br>

### stat

```stat``` ek file ya directory ke bare mein detailed information deti hai jaise size, permissions, last accessed time, modified time, inode number, etc. Ye ```ls -l``` se zyada detail deti hai.

Syntax:
```
stat filename
```

Examples:
- File ka detail dekhna:
```
stat file.txt
```

- Directory ka detail dekhna:
```
stat /etc
```

Real-World Use Case: Tumhe ye check karna hai ki ek config file last time kab modified hui thi, to stat config.yaml useful hai.

<br>

### basename

Ye command ek path se filename extract karne ke liye hoti hai.

Syntax:
```
basename path
```

Examples:
- Path se filename nikalna:
```
basename /home/testusr/file.txt
```
Output:
```
file.txt
```

Real-World Use Case: Agar tum automation script likh rahe ho aur tumhe full path se sirf file ka naam chahiye ho, ```basename``` best hai.

<br>

### dirname

Ye command ek path se directory name extract karne ke liye hoti hai (opposite of ```basename```).

Syntax:
```
dirname path
```

Examples:
- Path se directory nikalna:
```
dirname /home/testusr/file.txt
```
Output:
```
/home/testusr
```

Script mein use:
```
DIR=$(dirname /var/log/nginx/error.log)
echo $DIR
```

Real-World Use Case: Backup script mein agar tumhe ek file ka directory location chahiye ho.

<br>

### rsync

```rsync``` ek bahut powerful command hai jo files aur directories ko local ya remote systems ke beech sync/transfer karti hai. Ye bandwidth efficient hai aur sirf differences transfer karti hai (incremental).

Syntax:
```
rsync [options] source destination
```

Common Options:
- ```a``` → archive mode (preserve permissions, symlinks, etc.).
- ```v``` → verbose (show details).
- ```z``` → compress data during transfer.
- ```e``` ssh → SSH ke through transfer karna

Examples:
- Local directory sync karna:
```
rsync -avz /home/testusr/ /backup/testusr/
```

- Remote server par files bhejna:
```
rsync -avz -e ssh /home/testusr/ user@192.168.1.10:/backup/
```

- Remote se local mein lana:
```
rsync -avz user@192.168.1.10:/backup/ /home/testusr/
```

- Sirf differences update karna (incremental backup):
```
rsync -av --delete /source/ /destination/
```

Real-World Use Case: Tum apne server ka daily backup remote server par bhejna chahte ho. ```rsync -avz -e ssh /var/www user@backup-server:/data/``` perfect hai.

