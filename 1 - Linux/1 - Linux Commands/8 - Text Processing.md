# Text Processing

<img src="https://drive.google.com/uc?export=view&id=1q1OkioQvR6mcabK8yaHhgbXQCjLVpLp8" width="550" height="820">

<br>

### cat

```cat``` ka full form hai **concatenate** matlab add karna.

Cat command ka sabse jyada use hota hai file ke ander ka content dekhna, matlab file main kya likha hua hai vo dekhna.

Cat ka main kaam hota hai — text files ko dekhna, banana, merge karna, aur content ko redirect karna.

<br>

**Syntax**:
```
cat [options] [file...]
```

<br>

**Examples**:

**Basic Usage – File ka content dekhna**:
```
cat filename.txt
```
Ye command ```filename.txt``` ke content ko terminal pe print karega.

```
cat /etc/hostname
```
Output:
```
ubuntu-server
```
- ```cat``` command file read kar ke uska text directly screen pe dikhata hai.
- Ye chhoti files dekhne ke liye best hai (badi files ke liye ```less``` ya ```more``` use karte hain).

**Multiple Files ko ek sath dekhna**:
```
cat file1.txt file2.txt
```
Ye dono files ke content ko ek sath print karega, jahan ```file1.txt``` ka content pehle aur ```file2.txt``` ka baad mein dikhayega.

**Output ko kisi file mein likhna (Redirection)**:

Agar tumhe ek nayi file banana hai ya kisi file mein data likhna hai:
```
cat > newfile.txt
```
Ab terminal khula rahega — kuch bhi likho:
```
This is a new file.
Created using cat command.
```
Ab Ctrl + D dabao (EOF - End Of File)
File save ho jaayegi.

**Existing file mein content append karna**:
```
cat >> existingfile.txt
```
Ab jo likhoge, wo file ke end mein add ho jaayega.

**File ke line numbers dikhana**:
```
cat -n filename.txt
```
Output:
```
1  This is line 1
2  This is line 2
3  This is line 3
```

<br>
<br>

### less

```less``` command file ke content ko page by page dikhata hai.

```less``` ek file viewer command hai jo terminal par bade text files ko scroll aur navigate karne ke liye use hota hai.

<br>

**Why use less?**
- ```cat``` chhoti files ke liye useful tha, lekin agar file bahut badi ho (jaise logs, config, code files, etc.), to cat sab kuch ek sath dump kar deta hai.
- ```less``` file ko paged view mein kholta hai — tum upar–neeche scroll, search, jump, aur navigate kar sakte ho.

<br>

**Syntax**:
```
less [options] filename
```

<br>

**Basic Navigation Keys (Important)**:

| Key           | Function                                |
| ------------- | --------------------------------------- |
| **Space**     | Next page (scroll down one screen)      |
| **b**         | Previous page (scroll up one screen)    |
| **Enter / ↓** | Scroll down one line                    |
| **↑**         | Scroll up one line                      |
| **g**         | Go to beginning of file                 |
| **G**         | Go to end of file                       |
| **/pattern**  | Search forward for “pattern”            |
| **?pattern**  | Search backward for “pattern”           |
| **n**         | Repeat last search (same direction)     |
| **N**         | Repeat last search (opposite direction) |
| **q**         | Quit (exit less)                        |

<br>

**Examples**:

**Example 1: File view karna**:
```
less /etc/passwd
```
- File open hoti hai.
- Tum scroll kar sakte ho.
- ```/root``` likhkar search kar sakte ho.
- ```q``` dabakar exit kar sakte ho.

**Example 2: Command output ke sath use karna (Piping)**:
```
kubectl logs my-pod-1234 | less
```
Pod logs scroll aur search karne ke liye perfect.

**Example 3: File ke end me directly open karna (useful for latest logs)**:
```
less +G /var/log/syslog
```
```+G``` means — directly jump to end of file.

**Example 4: Specific line number pe jump karna**:
```
less +45 file.txt
```
Direct line 45 se file open karega.

<br>
<br>

### more

```more``` command bhi text file ke content ko page by page dekhne ke liye use hoti hai, jaise ```less``` command use hoti hai.

Lekin ```less``` command jyada fast aur useful hai ```more``` se.

**Difference Between more and less**:

| Feature                    | `more`                                    | `less`                             |
| -------------------------- | ----------------------------------------- | ---------------------------------- |
| Backward scrolling         | ❌ Not available (except one page via `b`) | ✅ Available                        |
| Search backwards           | ❌ No                                      | ✅ Yes                              |
| Memory usage               | Light                                     | Heavier                            |
| Performance on large files | Slower                                    | Faster                             |
| Modern usage               | Outdated                                  | Preferred (almost replaces `more`) |


<br>
<br>

### head

```head``` ek file content viewer command hai jo kisi file ke starting lines (by default 10 lines) display karta hai.

Ye command file ke content ko display karne ke liye use hoti hai lekin ye file ke shuru ke line dikhati hai.

Yani jab tumhe sirf beginning portion dekhna ho kisi text file, log file ya configuration file ka — tab tum ```head``` use karte ho.

<br>

**Syntax**:
```
head [options] [file_name]
```

Example:
```
head /etc/passwd
```
Ye ```/etc/passwd``` file ki pehli 10 lines terminal par print karega.

<br>

**Real Life Analogy**:

Socho tumhare paas 1GB ka log file hai (```application.log```) aur tumhe bas ye dekhna hai ki start me kaunsi service ne initialize kiya, ya file ka format kaisa hai —
poora file open karna time waste hai, to tum ```head``` se uske starting 10 lines dekh loge.

<br>

**Default Behavior**:
- Agar koi option nahi diya gaya ho, to 10 lines show karta hai.
- Agar multiple files specify karte ho, to har file ke content ke aage file name header likhta hai.

Example:
```
head file1.txt file2.txt
```
Output:
```
==> file1.txt <==
<file1 ke first 10 lines>

==> file2.txt <==
<file2 ke first 10 lines>
```

<br>

**Common Options**:

| Option   | Description                                                | Example                                     |
| -------- | ---------------------------------------------------------- | ------------------------------------------- |
| `-n`     | Specific number of lines show karega                       | `head -n 5 file.txt` (5 lines)              |
| `-c`     | Specific number of bytes show karega                       | `head -c 20 file.txt` (first 20 bytes only) |
| `-v`     | Always print headers (useful with multiple files)          | `head -v file1.txt file2.txt`               |
| `-q`     | Suppress file headers (no header shown for multiple files) | `head -q file1.txt file2.txt`               |
| `--help` | Help message show karega                                   | `head --help`                               |


<br>

**Examples**:

**1. Show default 10 lines**:
```
head /var/log/syslog
```
Ye system log ki pehli 10 lines dikhata hai.

**2. Show first 20 lines**:
```
head -n 20 access.log
```
Useful jab tumhe thoda zyada context chahiye ho log file ke start ka.

**3. Show first 50 bytes (not lines)**:
```
head -c 50 file.txt
```
Ye sirf first 50 characters (bytes) dikhata hai, chahe line complete ho ya nahi.

**5. Suppress file name headers**:
```
head -q file1.txt file2.txt
```
Header (==> filename <==) nahi aayega — direct lines print honge.

**6. Command output ke saath use karna (Piping)**:
```
ps aux | head
```
Ye system ke running processes me se pehle 10 processes dikhata hai. Yani jab tum command ka output limit karna chaho, ```head``` bahut kaam ka hai.

<br>
<br>

### tail

```tail``` ek Linux command-line utility hai jo kisi file ke last lines (end portion) ko display karti hai.

Default behavior me ye last 10 lines show karti hai.

Matlab agar file bohot badi hai (jaise log files, CSVs, reports), aur tumhe sirf latest entries ya recent logs dekhne hain — to ```tail``` command best hai.

<br>

**Syntax**:
```
tail [options] [file_name]
```

Example:
```
tail /var/log/syslog
```
Ye system log file (```/var/log/syslog```) ke aakhri 10 lines dikhata hai.

<br>

**Default Behavior**:
- Agar tum bina option ke ```tail``` likhoge, to 10 last lines show karega.
- Ye reverse print nahi karta — sirf end portion ka latest part dikhata hai.
- File ke andar pointer end se start hota hai aur upar ki direction me data read karta hai.

<br>

**Commonly Used Options**:

| Option        | Description                                       | Example                             | Explanation                                                          |
| ------------- | ------------------------------------------------- | ----------------------------------- | -------------------------------------------------------------------- |
| `-n <number>` | Specific number of lines show karne ke liye       | `tail -n 20 /var/log/messages`      | Last 20 lines show karega                                            |
| `-c <bytes>`  | Specific bytes show karne ke liye                 | `tail -c 50 file.txt`               | File ke last 50 bytes dikhata hai                                    |
| `-f`          | Follow mode (real-time updates)                   | `tail -f access.log`                | File ke end me agar new line add hoti hai to live update show karega |
| `-F`          | Similar to `-f`, but reconnects if file recreated | `tail -F /var/log/nginx/access.log` | Useful when log file rotate hoti hai                                 |
| `--pid=<pid>` | Process ke khatam hone tak output follow karega   | `tail --pid=1234 -f file.log`       | Jaise hi process ID `1234` end hoti hai, tail stop ho jata hai       |
| `-q`          | Quiet mode — file headers suppress karta hai      | `tail -q file1 file2`               | Multiple file output me header print nahi hota                       |
| `-v`          | Verbose mode — hamesha header print karta hai     | `tail -v file1 file2`               | Har file ke output ke aage filename print karega                     |


<br>

**Examples**:

**Example 1: Default 10 last lines**:
```
tail file.txt
```
Ye file ke last 10 lines terminal pe show karega.

**Example 2: Specific number of lines (e.g., 15)**:
```
tail -n 15 /var/log/syslog
```
System log ke last 15 entries show karega.

**Example 3: Last 100 bytes show karna**:
```
tail -c 100 file.txt
```
File ke aakhri 100 characters/bytes show karega (chahe wo half line ho ya full).

**Example 4: Real-time monitoring (DevOps ke liye sabse common)**:
```
tail -f /var/log/nginx/access.log
```
Ye continuously file ke end ko monitor karega. Jaise hi naye logs likhe jaayenge, wo terminal pe live show honge. Bahut useful jab tum web server, database, ya application ke logs real-time me dekhna chahte ho.

**Example 5: File rotation ke case me auto-reconnect**:
```
tail -F /var/log/nginx/error.log
```
Agar log file rotate ho gayi (purani delete aur nayi create ho gayi), tail -F automatically nayi file ko follow karega. Isliye -F production me -f se better hota hai.

**Example 7: Tail + Pipe combination (Powerful use)**:
```
journalctl -u nginx | tail -n 20
```
Systemd logs me se sirf last 20 lines dikhata hai.

<br>
<br>

### grep

```grep``` ka full form hai: **Global Regular Expression Print**.

Ye ek command-line text searching tool hai jo kisi file (ya stream) me pattern match karta hai aur matching lines print karta hai.

Matlab: ```grep``` kisi text ya log file ke andar specific word, phrase, ya pattern search karta hai.

<br>

**Syntax**:
```
grep [options] pattern [file...]
```

Example:
```
grep "error" /var/log/syslog
```
Ye command ```/var/log/syslog``` file me "error" word ko search karega aur jahan-jahan mila hai wo line print karega.

<br>

**Simple Understanding**:

Socho tumhare paas ek bada log file hai — ```app.log```, Aur tumhe dekhna hai ki “database” word kaha-kaha aaya hai. Poora file manually dekhna mushkil hai, to tum simply likhte ho:
```
grep "database" app.log
```
Aur ```grep``` tumhe sirf wahi lines print karega jisme “database” likha hoga. Yani non-matching lines hide ho jaayengi.

<br>

**Commonly Used Options**:

| Option         | Description                                    | Example                            | Explanation                                  |                   |
| -------------- | ---------------------------------------------- | ---------------------------------- | -------------------------------------------- | ----------------- |
| `-i`           | Case-insensitive search                        | `grep -i "error" file.txt`         | “Error”, “ERROR”, “eRrOr” sab match honge    |                   |
| `-v`           | Invert match (jo match **nahi** hote)          | `grep -v "info" file.txt`          | Info ke alawa sab print karega               |                   |
| `-r`           | Recursive search (folders ke andar)            | `grep -r "timeout" /etc`           | Subdirectories ke files me bhi search karega |                   |
| `-n`           | Line numbers ke saath output                   | `grep -n "main" code.py`           | Matching lines ke line numbers show karega   |                   |
| `-c`           | Sirf count of matches                          | `grep -c "error" app.log`          | Kitni baar “error” aaya hai                  |                   |
| `-l`           | Sirf filenames jaha match mila                 | `grep -l "error" *.log`            | Bas file names print karega                  |                   |
| `-H`           | File name show kare (useful in multiple files) | `grep -H "failed" *.log`           | File name ke saath matching line             |                   |
| `-A <num>`     | Match ke baad <num> lines bhi dikhaye          | `grep -A 3 "failed" log.txt`       | Matching line + next 3 lines                 |                   |
| `-B <num>`     | Match ke pehle <num> lines bhi dikhaye         | `grep -B 2 "failed" log.txt`       | Previous 2 lines + matching line             |                   |
| `-C <num>`     | Match ke aage-piche dono side ki lines         | `grep -C 2 "error" file.txt`       | 2 lines before and after                     |                   |
| `-E`           | Extended regex allow karta hai (ERE)           | `grep -E "error                    | failed"`                                     | Multiple patterns |
| `-F`           | Fixed string search (no regex)                 | `grep -F "if(x>5)" file.txt`       | Regex characters ignore karega               |                   |
| `--color=auto` | Matches ko highlight kare                      | `grep --color=auto "fail" log.txt` | Colored output                               |                   |


<br>

**Examples**:

**Example 1: Basic search**:
```
grep "root" /etc/passwd
```
Output me wo saari lines aayengi jisme “root” likha hai.

**Example 2: Case-insensitive search**:
```
grep -i "error" /var/log/syslog
```
“error”, “Error”, “ERROR” sab match honge.

**Example 3: Invert match (non-matching lines)**:
```
grep -v "INFO" app.log
```
“INFO” wali lines skip ho jaayengi — useful jab unwanted entries ignore karni ho.

**Example 4: Recursive search**:
```
grep -r "Listen" /etc/nginx/
```
Nginx configuration ke har file me “Listen” directive search karega.

**Example 5: Show line number**:
```
grep -n "server" /etc/nginx/nginx.conf
```
Output me har match ke saath line number dikhata hai — useful for debugging.

**Example 6: Count number of matches**:
```
grep -c "404" /var/log/nginx/access.log
```
Kitni baar 404 error aaya — ek single number me output karega.

**Example 7: Regex (Regular Expression) use karna**:
```
grep -E "error|failed|critical" app.log
```
Ye 3 alag words me se koi bhi match mila to wo line print karega.

<br>
<br>

### sed
