# Tracking path changes

<img src="https://drive.google.com/uc?export=view&id=1VUKeBh9GGBFFene8-8V76f-QGiKB7z7h" width="450" height="320">

<br>

### Pehle concept samjho — “Tracking Path Changes” kya hota hai?

Git normal text changes to track karta hi hai, balki files ki position (path) aur presence bhi track karta hai.

Iska matlab:
- Agar tum ek file delete karte ho → Git track karega ki ye file remove ho gayi.
- Agar tum ek file rename/move karte ho → Git track karega ki ye file ek jagah se dusri jagah gayi.

Issi liye humein special commands milte hain:

<br>

### git rm [file]

Kya karta hai:
- Ye command tumhare working directory se file ko delete kar deta hai aur us deletion ko staging area me add kar deta hai.
- Matlab file remove bhi ho gayi aur agla commit karte hi history me ye deletion aa jayega.

**Important**:
- Sirf ```rm file.txt``` (Linux ka command) file delete karega par Git ko nahi batayega.
- ```git rm file.txt``` file ko delete karega aur Git ko batayega ki ye change track karna hai.

Example:
```
git rm oldfile.txt
```
Ouput:
```
rm 'oldfile.txt'
```

Ab ```git status``` me dikhega:
```
deleted: oldfile.txt
```

Agla step:
```
git commit -m "Remove oldfile.txt"
```

Ab tumhare repo ki history me oldfile.txt remove ho chuki hai.

**Tip**:
- ```git rm --cached file.txt``` = file ko Git tracking se hatao par working directory me rakho (useful for accidentally committed files).
- ```git rm -r folder/``` = pura folder hatao.

<br>

### git mv [existing-path] [new-path]

Kya karta hai:
- Ye command ek file ko rename/move karta hai aur us change ko staging area me dal deta hai.
- Matlab tumne ek jagah se dusri jagah file shift ki aur Git ko turant pata chal gaya.

Example: rename file
```
git mv oldname.txt newname.txt
```
Output:
```
Renaming oldname.txt to newname.txt
```

Ab ```git status``` me dikhega:
```
renamed: oldname.txt -> newname.txt
```

Agla step:
```
git commit -m "Rename oldname.txt to newname.txt"
```

Ab history me rename track ho gaya.

<br>

Example: move file to folder
```
git mv scripts/app.py src/app.py
git commit -m "Move app.py from scripts to src"
```

Ab history me app.py ka path change track ho gaya.

**Note**:
- Tum chaaho to manually bhi ```mv``` (OS ka command) se file move karke ```git add``` / ```git rm``` kar sakte ho. Lekin ```git mv``` sab ek saath kar deta hai.

