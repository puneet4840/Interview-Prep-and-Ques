# Setup and Init Commands

<img src="https://drive.google.com/uc?export=view&id=1gUI3n-7BQSNMTHKSh51GY_AqqRp1pkBL" width="400" height="160">

<img src="https://drive.google.com/uc?export=view&id=1Qg9945w--E7mbVdDzlEh3FvweDE-kWO4" width="100" height="160">

<br>
<br>

### git config ka basic matlab

```git config``` Git ka configuration command hai. Isse tum Git ki settings set kar sakte ho, jaise:
- User ka naam & email.
- Merge tool.
- Default editor.
- Color settings.

… aur bhi bohot kuch.

Git ki configuration teen level par hoti hai:

| Level      | File                                     | Scope                              |
| ---------- | ---------------------------------------- | ---------------------------------- |
| **System** | `/etc/gitconfig`                         | Sab users ke liye system-wide      |
| **Global** | `~/.gitconfig` ya `~/.config/git/config` | Current user ke liye sab repos par |
| **Local**  | `.git/config` (repo ke andar)            | Sirf ek repo ke liye               |

- ```--global``` = user level config.
- ```--local (default)``` = current repo config.

<br>
<br>

**git config --global user.name "[firstname lastname]"**:

Kya karta hai:
- Git ko batata hai ki tumhara naam kya hai.
- Jab tum commits karoge to commit metadata me “Author: <naam>” likha aayega.
- Is naam ko GitHub/GitLab PR me credit dene ke liye use karta hai.

Syntax:
```
git config --global user.name "[firstname lastname]"
```

Components:
- ```git config``` = config command.
- ```--global``` = current user ke liye sabhi repos par ye naam apply ho

Example:
```
git config --global user.name "Puneet Verma"
```

Ab jab bhi tum commit karoge, git log me aisa dikhega:
```
commit 3d6f2ab
Author: Puneet Verma <puneet@example.com>
Date:   Thu Sep 26 20:10 2025

    Added new feature
```

<br>
<br>

**git config --global user.email "[valid-email]"**

Kya karta hai:
- Git ko batata hai ki tumhara email address kya hai.
- Ye commit metadata me “Author: <email>” me dikhta hai.
- GitHub tumhare commits ko tumhare account se link karne ke liye isi email ka use karta hai.

Components:
- ```--global``` = current user ke liye sabhi repos par ye naam apply ho

Example:
```
git config --global user.email "puneet@gmailc.om"
```

Ab jab tum commit karoge, git log me:
```
commit 4e7g2cd
Author: Puneet Verma <puneet.verma@example.com>
Date:   Fri Sep 27 09:00 2025

    Fixed bug in login flow
```

<br>
<br>

**git config --global color.ui auto**

Kya karta hai:
- Git ke output me automatic coloring enable karta hai.
- Jaise ```git status```, ```git diff```, ```git log``` me changes red/green/yellow me dikhte hain.
- Ye readability improve karta hai taaki tum turant samajh sako kya change hai.

Components:
- ```auto``` = Git khud decide kare kab colors dikhane hain (jab terminal support kare).

Example:
```
git config --global color.ui auto
```

Ab git status likhne par:
- Untracked files = red
- Modified files = yellow
- Staged files = green

Agar colouring off karna ho to:
```
git config --global color.ui false
```

<br>
<br>

**Config ko verify karne ke liye**

Example:
```
git config --list
```

Output:
```
user.name=Puneet Sharma
user.email=puneet.sharma@example.com
color.ui=auto
```
