# Stage and Commit commands

<br>

<img src="https://drive.google.com/uc?export=view&id=19kbTqSoN3pwgrAUNqjim66vfn12WWx4W" width="350" height="520">

<br>

### git status

Tumhare working directory aur staging area ka current status dikhata hai, jaise:
- Kaunse files modify hui hain par staged nahi hain.
- Kaunse files staging area mein hain commit ke liye.
- Kaunse files untracked hain (nayi files jo Git ko nahi pata).

Example:
```
git status
```
Output:
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   app.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
        modified:   readme.md

Untracked files:
        newfile.txt
```

Is output se tum turant samajh sakte ho kya staged hai aur kya nahi.

<br>
<br>

### git add [file]

- Files ko working directory se staging area main leke jata hai.
- Staging area vo area hota hai jaha files ko next commit karne ke liye ready rekhte hain. Files ko directly working directory se commit nahi kar sakte hain. Staging are isliye hota hai ki aap decide kar sako konsi file commit karni hai aur konsi nahi.
- Matlab ab Git ko pata hai ki agla commit banega to is file ka ye version use karna hai.

Syntax:
```
git add [filename]
```

Example:
```
git add text.py
```

Agar working directory ki sabhi file ko stage karna hai hai to:
```
git add .
```

Note:
- Agar tumne add karne ke baad file fir edit ki to staging area me purana version hai, naya add karne ke liye dobara git add karna padega.

<br>
<br>

