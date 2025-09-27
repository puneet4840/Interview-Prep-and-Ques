# Share and Update

<img src="https://drive.google.com/uc?export=view&id=1RQQAx34O_5PKHYwIdLhMYxBiHWd9KAPu" width="450" height="320">

<br>

### Pehle Concept samjho— “Share & Update” kya hota hai?

Git ek distributed version control system hai. Matlab har developer ke paas repo ka full copy hota hai. Par jab hum GitHub jaisi jagah par repo rakhte hain, use remote repository bolte hain.

Local aur remote ke beech sync karne ke liye hum in commands ka use karte hain.

### git remote add [alias] [url]

Kya karta hai:
- Ye command tumhare local repo ko ek remote repo se connect karta hai. Matlab remote repository ke url ko local git main store kar deta hai jisse local se remote repo pe code ko push ya pull kiya ja sake.
- ```alias``` ek naam hota hai jo tum remote ko dete ho (jaise ```origin```).
- ```url``` woh address hota hai jahan tumhari remote repo host hai.

Example:
```
git remote add origin https://github.com/puneet/myrepo.git
```
- ```origin``` alias hai. Hum ek naam dete hain remote repo ko.

Check karne ke liye ki kitne remote repo hain:
```
git remote -v
```
Output:
```
origin  https://github.com/puneet/myrepo.git (fetch)
origin  https://github.com/puneet/myrepo.git (push)
```

Ab tumhara local repo remote se linked hai.

<br>
<br>

### git fetch [alias]

alias = remote repo ka naam jo apke local git main store hai, jaise ```origin```

Kya karta hai:
- Remote repo se latest commits aur branches download karta hai, par unko local working branch me merge nahi karta.
- ```git fetch``` remote repository se lastest code changes leke aata hai, lekin local repository main un changes ko merge nhi karta hai.
- ```git fetch``` remote repository se sirf naye commits (updates) laata hai, lekin inhe turant aapke current branch ya working directory me apply nahi karta.
- Matlab, changes sirf local repository ke "remote-tracking branch" me store ho jaate hain.
- Aap review kar sakte hain ki remote branch par kya changes aaye hain, aur jab chahe tab manually merge ya rebase kar sakte hain.

Example:
```
git fetch origin
```

Ye karega:
- GitHub par ```origin``` remote se latest commits, branches ka data le aayega.
- Tum ```git branch -r``` se remote branches dekh sakte ho.
- Tumhare local branch me koi automatic merge nahi hoga.

<br>

### git merge [alias]/[branch]

Kya karta hai:
- Tumhare local branch me remote branch ke changes merge karta hai.
- Ye step tab hota hai jab tumne ```git fetch``` kiya ho aur ab wo changes apni local branch me lana chahte ho.

Example:
```
git checkout main        # ensure you are on main
git fetch origin         # get latest from remote
git merge origin/main    # merge remote main into local main
```
Output:
```
Updating a1b2c3..d4e5f6
Fast-forward
 file1.txt | 2 ++
 1 file changed, 2 insertions(+)
```

Ab tumhara local ```main``` branch remote ke saath updated hai.

<br>

### git push [alias] [branch]

Kya karta hai:
- Tumhare local branch ke commits ko remote branch par bhejta hai.
- Matlab jo changes tumne locally commit kiye, wo ab remote par bhi save ho jayenge.

Example:
```
git push origin main
```

Ye karega:
- Tumhare local ```main``` ke commits GitHub par ```main``` branch me push kar dega.

Agar remote par branch nahi hai, to Git ek nayi branch create kar dega (agar permission hai).

<br>

### git pull

Kya karta hai:
- Ye command remote branch se changes fetch karega aur unko tumhare local branch me merge bhi karega.
- Ye ek shortcut hai jo git fetch + git merge dono ek saath karta hai.

Example:
```
git pull origin main
```

Ye karega:
- ```origin``` remote se ```main``` branch ke changes ko fetch karega.
- Tumhare current local branch me merge karega.

Output:
```
Updating 45ae91c..9d3b21f
Fast-forward
 newfile.txt | 3 +++
 1 file changed, 3 insertions(+)
```

Ab tumhara local branch remote ke saath sync ho gaya.

<br>
<br>

### git pull VS git fetch

Git fetch aur git pull dono commands remote repository se changes lane ke liye use hoti hain, lekin dono ka kaam alag hai.

**git fetch**:
- ```git fetch``` remote repository se sirf naye commits (updates) laata hai, lekin inhe turant aapke current branch ya working directory me apply nahi karta.
- Matlab, changes sirf local repository ke "remote-tracking branch" me store ho jaate hain.
- Aap review kar sakte hain ki remote branch par kya changes aaye hain, aur jab chahe tab manually merge ya rebase kar sakte hain.

Example:

Agar aapne ```git fetch origin``` kiya, toh remote par jo bhi naye commits hain, woh aapke local repo me aajayenge lekin aapke branch me nahi dikhengi. Aap ```git log origin/main``` se dekh sakte hain ki remote par kya changes hain.


<br>

**git pull**:
- ```git pull``` do commands ka combo hai: pehle git fetch karta hai, phir turant changes ko merge bhi kar deta hai aapke current branch me.
- Matlab, remote par jo bhi update hai, woh turant aapke local branch me aa jaata hai.
- Isse aapka branch remote ke latest version ke saath sync ho jaata hai.

Example:

Agar aap ```git pull origin main``` karte hain, toh jitne bhi naye commits remote ki main branch par hain, woh fetch ho kar aapki local main branch me merge ho jaayenge.

<br>

**Kab git fetch use karna chahiye aur kab git pull**:

Kab git fetch use karein:
- Jab aap remote repository ke changes pehle dekhna chahte hain, bina apne branch me merge kiye.
- Team me bahut log kaam kar rahe hain aur aap conflicts se bachna chahte hain—fetch karke dekh sakte hain kya-kya naye commits aaye hain, fir decide kar sakte hain unko kaise merge karna hai.

Kab git pull use karein:
- Jab aap chahte hain ke remote ke saare latest changes turant aapke local branch me merge ho jaayein.
- Jab aap changes ko bina review kiye bhi direct update kar sakte hain ya aapko pata hai ki koi conflict nahi hoga, tab pull karna sahi hai.
