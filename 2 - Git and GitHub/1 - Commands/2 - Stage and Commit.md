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

### git reset [file]

- Matlab tumne galti se file ko git add kar diya tha aur commit nahi karna chahte abhi, to tum unstage kar sakte ho.
- Ye command files ko staging area se working directory main daal deti hai, matlab unstage kar deti hai.

```git reset``` purani command hai, naye version main ```git restore --staged <file>``` command ka use hota hai.

Example-1:
```
git reset app.py
```

Example-2:
```
git restore --staged app.py
```

<br>
<br>

### git diff

Kya karta hai:
- Tumhare working directory aur staging area ke beech ka difference dikhata hai.
- Matlab tumne file main ek nayi line add ki hai lekin usko tumne stage nahi kiya hai, ye difference dikhata hai.

Syntax:
```
git diff
```

Example:
```
git diff
```

Output:
```
diff --git a/app.py b/app.py
index 989e3dd..49b5363 100644
--- a/app.py
+++ b/app.py
@@ -1 +1 @@
-print("Hello, Puneet")
\ No newline at end of file
+print("Hii, Puneet")
\ No newline at end of file
```

Ye dikhata hai ki tumne nayi line add ki hai lekin ```git add``` nhi kiya hai.

Example-2:

Suppose mere ```app.py``` file created hai, jisme likha hai:
```
print("Hello, Puneet")
```

Isko maine stage karke commit kar diya. Commit ke baad git isko track kar rha hai, Commit karne ke baad maine is file main changes kiye, jaise:
```
print("Hii, Puneet")
```

Agar mai ```git diff``` command run karunga to mujhe output milega:
```
diff --git a/app.py b/app.py
index 989e3dd..49b5363 100644
--- a/app.py
+++ b/app.py
@@ -1 +1 @@
-print("Hello, Puneet")
\ No newline at end of file
+print("Hii, Puneet")
\ No newline at end of file
```

Iska matlab hai aapne file main kuch change kiya hai lekin us file ko staging area main nahi daala hai, matlab ```git add``` nhi kiya hai.

<br>
<br>

### git diff --staged (ya git diff --cached)

Kya karta hai:
- Tumhare staging area aur last commit ke beech ka difference dikhata hai.
- Matlab jo tumne stage kar diya hai (```git add``` kar diya) par abhi commit nahi kiya, unka diff.
- Matlab tumne file main last commit main kya likha tha aur ab commit karne ja rhe ho.

Syntax:
```
git diff --staged
```

Example-1:
```
git diff --staged
```

Output:
```
diff --git a/app.py b/app.py
index 989e3dd..49b5363 100644
--- a/app.py
+++ b/app.py
@@ -1 +1 @@
-print("Hello, Puneet")
\ No newline at end of file
+print("Hii, Puneet")
\ No newline at end of file
```

Example-2:

Suppose mere ```app.py``` file created hai, jisme likha hai:
```
print("Hello, Puneet")
```

Isko maine commit kar diya, ab maine file main kuch changes kiye:
```
print("Hii, Puneet")
```

Uper changes karne ke baad maine file ko ```git add``` yani staged kar diya, Agar main ab ```git diff --staged``` command run karunga output main pehle commit kab kiya the to file main kya likah tha aur ab commit ke baad changes karke file ko stage kiya hai usme kya likhe hai:
```
diff --git a/app.py b/app.py
index 989e3dd..49b5363 100644
--- a/app.py
+++ b/app.py
@@ -1 +1 @@
-print("Hello, Puneet")
\ No newline at end of file
+print("Hii, Puneet")
\ No newline at end of file
```

Isse tum verify kar sakte ho ki commit hone wala hai kya.

<br>
<br>

### git commit -m "[descriptive message]"

Kya Karta Hai:
- Tumhare staged changes ko naya commit (snapshot) bana deta hai.
- Commit me tumhara naam, email, date/time aur message hota hai.

Components:
- ```-m```: message
- ```[descriptive message]``` = change ka short description.

Example:
```
git commit -m "Added new feature to app.py"
```

Output:
```
[main 3b5d9f1] Added new feature to app.py
 1 file changed, 1 insertion(+)
```

Ab tum git log me naya commit dekh sakte ho.

Note:
- Commit tabhi banta hai jab tumne git add se changes stage kiye ho.
- Agar tum bina stage kiye commit karoge to Git bolega “nothing to commit”.

