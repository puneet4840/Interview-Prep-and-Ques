# Branch and Merge

<img src="https://drive.google.com/uc?export=view&id=15cNMCAk3PBB53GNI44h4-Weq_Lv81Co-" width="450" height="320">

<br>
<br>

### git branch

Kya karta hai:
- Tumhare repository ki saari branches list karta hai.
- Jis branch par tum abhi ho uske aage ```*``` hota hai. ```*``` current branch ko represent karta hai.

Example:
```
git branch
```
Output:
```
* main
  feature/login
  bugfix-issue12
```

Yahan tum ```main``` branch me ho, aur tumhare paas do aur branches hain.

<br>

### git branch [branch-name]

Kya karta hai:
- Current commit se ek nayi branch banata hai. Matlab jis branch main tum hote ho usi se ek nayi branch create kar deta hai.

Example:
```
git branch develop
```

Output:
```
git branch

* main
  develop
```

Ab tumhare paas ek feature-login branch hai par tum abhi bhi ```main``` me ho. Branch banane se tum automatically usme switch nahi hote.

**Tip**: Tum ```git checkout -b branchname``` se ek step me branch create kar sakte ho + usme switch kar sakte ho.

Example:
```
git checkout -b feature/login
```
Output:
```
git branch

* feature/login
  main
```

<br>

### git checkout

Kya karta hai:
- Tumko ek branch se dusri branch par switch karta hai.
- Tumhare working directory me us branch ka content aa jata hai (files, changes).

Example:
```
git checkout feature/login
```
Output:
```
Switched to branch 'feature-login'
```

Ab tum ```feature-login``` branch par ho aur jo bhi commit karoge us branch ki history me jayega.

**Tip**: Git 2.23+ me ye kaam ```git switch branchname``` se bhi hota hai (clearer command).

Example:
```
git switch develop
```
Output:
```
Switched to branch 'develop'
```

<br>

### git merge [branch]

Kya karta hai:
- Ek branch ko dusri branch me merge karta hai.
- Jis branch par tum ho usme kisi aur branch ke commits ko merge kar deta hai.
- Matlab tum apne current branch me doosri branch ke changes lekar aa rahe ho.

Example:
- Pehle ```main``` branch me aao:
```
git checkout main
```
- Fir feature branch ko main branch me merge karo:
```
git merge feature/login
```

Output:
```
Updating 8f8a63d..f3b0b9a
Fast-forward
 app.py | 5 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)
```

Ab ```main``` branch me ```feature/login``` ke saare commits aa gaye.

<br>

### git branch -d [branch-name]

Kya karta hai:
- Ye branch ko delete karta hai.
- Agar apki branch dusri branch me merge ho gyi hai to usko delete kar dega.

Example:
```
git branch -d develop
```
Output:
```
Deleted branch develop (was 13a2688).
```

**Tip**: Agar branch ko force-fully delete karna chahte hain hai to ```git branch -D <branch-name>``` command ka use karo.

<br>

### git log

Kya karta hai:
- Ye branch me hue commit ko dikhata hai.
- Tumhare current branch ki commit history dikhata hai. Ki branch me kitne commit hue hain.
- Ye tumhe samajhne me madad karta hai ki kya merge hua, kya naya commit hai.

Example:
```
git log
```
Output:
```
commit e4dabbd3f4922296c488dba2028432c6b3ef03e5 (HEAD -> master)
Author: Puneet Verma <puneet.verma02@nagarro.com>
Date:   Sat Sep 27 16:17:09 2025 +0530

    commnet added

commit 121567d87e4c7da57d0fad1449546a767ae08079
Author: Puneet Verma <puneet.verma02@nagarro.com>
Date:   Sat Sep 27 16:15:58 2025 +0530

    app added
```

**Note**: Commit logs main HEAD batata hai ki abhi latest commit konsa hua hai.

**Tip**:
-  Agar commit ek line main dekhne hain to ```git log --oneline``` command ka use karo.

Example:
```
git log --oneline
```
Output:
```
e4dabbd (HEAD -> master) commnet added
121567d app added
13a2688 new file added
91c4285 deleted
```
