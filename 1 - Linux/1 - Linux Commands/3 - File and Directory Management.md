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
