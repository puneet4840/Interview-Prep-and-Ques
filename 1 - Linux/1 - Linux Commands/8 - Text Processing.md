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

```sed``` ka full form hai **Stream Editor**.

Ye command text transformation ke liye use kiya jata hai.

```sed``` ek aisa command-line text processing tool hai jo input stream (file ya STDIN) me se text read karta hai, process karta hai (edit/replace/delete/insert), aur output print karta hai — bina original file ko modify kiye (jab tak hum explicitly na bole).

**Stream Editor ka Matlab**:
- “Stream” ka matlab hai data ka flow — jaise file ka content ya pipeline se aane wala input.
- “Editor” ka matlab hai — text ke andar changes karna (replace, delete, insert, etc).

<br>

**Syntax**:
```
sed [options] 'command' file
```

Example:
```
sed 's/root/admin/' /etc/passwd
```
- Isme ```s``` ka matlab hai substitute.
- Matlab “root” ko “admin” se replace karega, aur output terminal pe dikhayega (original file me change nahi karta).

<br>

**Sed ka Working Flow**:
- ```sed``` ek file (ya STDIN) read karta hai.
- Har line ko ek “pattern space” me load karta hai.
- Command apply karta hai (like replace, delete, print).
- Output print karta hai.
- Next line pe move hota hai.
- Jab tak file khatam nahi hoti — ye cycle chalta rehta hai.

Isliye isko “stream editor” kaha jata hai — kyunki ye continuously text stream process karta hai.

<br>

**Sed ke Important Options**:

| Option      | Description                                                       |
| ----------- | ----------------------------------------------------------------- |
| `-n`        | Suppress automatic printing (sirf jab `p` diya ho tab print kare) |
| `-e`        | Multiple sed commands specify karne ke liye                       |
| `-f`        | Sed script file se commands read kare                             |
| `-i`        | File me direct edit kare (in-place edit)                          |
| `-r` / `-E` | Extended regular expressions enable kare                          |


<br>

**Practical Examples (Step-by-Step)**:

**Example 1: Replace a word (first occurrence)**:
```
sed 's/Linux/Unix/' file.txt
```
Har line me sirf pehla “Linux” replace karega.

**Example 2: Replace all occurrences**:
```
sed 's/Linux/Unix/g' file.txt
```
Har line me “Linux” ke saare occurrences replace honge (because of ```g```).

**Example 3: Case-insensitive replacement**:
```
sed 's/linux/Unix/Ig' file.txt
```
“linux”, “LINUX”, “LiNuX” sab match karega (because of ```I```).

**Example 4: Replace only in specific line number**:
```
sed '5s/error/warning/' log.txt
```
Sirf line number 5 me “error” ko “warning” se replace karega.

**Example 5: Delete specific line**:
```
sed '3d' file.txt
```
3rd line delete kar dega output me.

**Example 6: Delete line range**:
```
sed '5,10d' file.txt
```
Line number 5 se 10 tak delete karega.

**Example 7: Print only matching lines**:
```
sed -n '/error/p' app.log
```
Sirf wo lines print karega jisme “error” likha hai.

**Example 8: Insert line before a match**:
```
sed '/error/i\# This is before error' app.log
```

**Example 9: Append line after a match**:
```
sed '/error/a\# This line added after error' app.log
```

**Example 10: Replace entire line on match**:
```
sed '/error/c\This line is replaced completely' app.log
```

**Example 11: Save changes to file**:
```
sed -i 's/old/new/g' file.txt
```
Original file modify karega (in-place). Without ```-i```, changes sirf terminal output me dikhti hain.


**Example 13: Backup ke saath edit**:
```
sed -i.bak 's/old/new/g' file.txt
```
Original file ka backup (```file.txt.bak```) bana lega.

<br>

**Real-Life DevOps Use Cases**:

**Replace configuration values in files**:
```
sed -i 's/port=8080/port=9090/' app.conf
```

**Remove commented lines**:
```
sed '/^#/d' nginx.conf
```

<br>

**Sed ke Commonly Used Commands**:

| Command               | Meaning                         | Example                                        | Explanation                                |
| --------------------- | ------------------------------- | ---------------------------------------------- | ------------------------------------------ |
| `s/pattern/replace/`  | Replace (substitute) pattern    | `sed 's/error/warning/' file.txt`              | “error” ko “warning” se replace            |
| `s/pattern/replace/g` | Replace all occurrences in line | `sed 's/error/warning/g' file.txt`             | Har line me saare “error” replace kare     |
| `p`                   | Print lines                     | `sed -n '5p' file.txt`                         | Sirf 5th line print kare                   |
| `d`                   | Delete line(s)                  | `sed '3d' file.txt`                            | 3rd line delete kare output me             |
| `q`                   | Quit after match                | `sed '/error/q' file.txt`                      | “error” milte hi stop kare                 |
| `a\text`              | Append line after match         | `sed '/error/a\This is after error' file.txt`  | “error” line ke baad nayi line insert kare |
| `i\text`              | Insert line before match        | `sed '/error/i\This is before error' file.txt` | “error” ke pehle line add kare             |
| `c\text`              | Replace entire line             | `sed '/error/c\This line replaced' file.txt`   | “error” line ko pura replace kare          |
| `y/set1/set2/`        | Transform characters (like tr)  | `sed 'y/abc/ABC/' file.txt`                    | a→A, b→B, c→C                              |
| `=`                   | Line numbers print kare         | `sed '=' file.txt`                             | Har line number print kare                 |


<br>
<br>

### awk

```awk``` ek text processing and data manipulation command hai.

```awk``` ek pattern scanning aur processing language hai — jise primarily structured text data (like logs, CSV, or space-separated text) ke saath kaam karne ke liye design kiya gaya hai.

Ye command ek text processing tool hai jo lines aur fields (columns) pe operations perform karta hai, Jaise:
- Pehle file ya input ko line by line read karta hai.
- Har line ko columns mein divide karta hai: Jaise ```$1``` pehla word/column hai, $2 dusra column, $0 poori line hoti hai. Jo bhi delimiter set kiya hai uske basis pe split hota hai.
- Pattern match karta ha: Tum specify kar sakte ho ki kaunsi condition ya regular expression pe action execute ho (jaise koi word aaye toh us line par kuch karo).
- Action perform karta hai: Jaise print, calculation, manipulations, line formatting, column extract, filtering, reporting.

<br>

**awk ka Full Form**:

“Aho, Weinberger, Kernighan”. Ye log iske creators the — isiliye iska naam bana AWK.

<br>

**Syntax**:
```
awk 'pattern { action }' file
```
| Part      | Description                                          |
| --------- | ---------------------------------------------------- |
| `pattern` | Condition ya matching expression                     |
| `action`  | Us line par kya karna hai (print, sum, modify, etc.) |
| `file`    | Input file jisme data hai                            |

Example:
```
awk '{ print $0 }' file.txt
```
```$0``` ka matlab poori line hota hai. Ye command file ki har line print karega (same as ```cat```).

<br>

**AWK ka Working Flow (Step-by-Step)**:
- File read karta hai line by line.
- Har line ko space ya defined separator ke base par fields (columns) me todta hai.
- Har field yaani columns ko variables ```$1```, ```$2```, ```$3```, … se represent karta hai.
- ```$0``` → poori line hoti hai.
- pattern match karta hai aur agar match hota hai to action execute karta hai.
- File ke end tak repeat karta hai.

<br>

**Important Variables**:

| Variable        | Meaning                                 |
| --------------- | --------------------------------------- |
| `$0`            | Puri current line                       |
| `$1, $2, $3, …` | Line ke fields (columns)                |
| `NR`            | Current line number (Number of Records) |
| `NF`            | Number of fields in current line        |
| `FS`            | Field Separator (default: space or tab) |
| `OFS`           | Output Field Separator (default: space) |
| `RS`            | Record Separator (default: newline)     |
| `ORS`           | Output Record Separator                 |
| `FILENAME`      | Current file ka naam                    |
| `BEGIN`         | Block executed before reading lines     |
| `END`           | Block executed after reading all lines  |


<br>

**Common awk Examples (Basic Level)**:

**Example 1: File print karna**:
```
awk '{ print }' file.txt
```
Har line print karega.

**Example 2: Specific columns print karna**:
```
awk '{ print $1, $3 }' employees.txt
```
Har line ka 1st aur 3rd column print karega. Matlab har line ka 1st aur 3rd word print karega.

**Example 3: File ke saath header add karna**:
```
awk 'BEGIN { print "Name\tAge" } { print $1, $2 }' data.txt
```
“BEGIN” block sirf ek baar run hota hai (file read hone se pehle).

**Example 4: Lines filter karna**:
```
awk '/error/' logfile.txt
```
Sirf wo lines print karega jisme “error” word aata hai.

**Example 5: Condition ke saath**:
```
awk '$3 > 80 { print $1, $3 }' marks.txt
```
Sirf un students ke naam aur marks print karega jinke marks 80 se jyada hain.

**Example 6: Line numbers print karna**:
```
awk '{ print NR, $0 }' file.txt
```
Har line ke aage uska line number add karega.

**Example 7: Column count print karna**:
```
awk '{ print NF, $0 }' file.txt
```
Har line ke starting me us line ke column count likhega.

<br>

**BEGIN aur END Block**:

Ye do blocks awk ke “special blocks” hain.
| Block   | Purpose                                                                            |
| ------- | ---------------------------------------------------------------------------------- |
| `BEGIN` | File read hone se **pehle** execute hota hai (initialization)                      |
| `END`   | File read hone ke **baad** execute hota hai (summary ya total print karne ke liye) |

<br>

**Example 1: BEGIN ke saath header**:
```
awk 'BEGIN { print "User\tUID" } { print $1, $3 }' /etc/passwd
```

**Example 2: END ke saath summary**:
```
awk '{ total += $2 } END { print "Total:", total }' sales.txt
```
Ye sabhi sales values ka total calculate karega.

<br>

**Custom Field Separator**:

By default ```awk``` space/tab ke base par fields split karta hai, lekin hum ```-F``` option se custom separator de sakte hain.

**Example 1: Colon-separated file (/etc/passwd)**:
```
awk -F: '{ print $1, $7 }' /etc/passwd
```
Username aur shell print karega.

**Example 2: CSV file**:
```
awk -F, '{ print $1, $3 }' employees.csv
```
CSV ke 1st aur 3rd columns print karega.

<br>

**Mathematical Operations**:

```awk``` arithmetic kar sakta hai jaise calculator:

Example:
```
awk '{ total = $2 + $3; print $1, total }' marks.txt
```
Columns 2 aur 3 ka sum karega aur naam ke saath print karega.

<br>

**Conditional Logic (if, else)**:

```awk``` if - else ka bhi use karta hai.

Example:
```
awk '{ if ($3 >= 60) print $1, "PASS"; else print $1, "FAIL" }' marks.txt
```
Agar marks >= 60, to “PASS”, warna “FAIL”.

<br>

**Pattern Matching**:
```
awk '$1 ~ /Puneet/ { print $0 }' users.txt
```
Sirf un lines ko print karega jinke pehle column me “Puneet” likha hai.

<br>

**String Functions**:

| Function           | Example                            | Description                   |
| ------------------ | ---------------------------------- | ----------------------------- |
| `length($1)`       | `awk '{ print length($1) }'`       | Column 1 ki length            |
| `toupper($1)`      | `awk '{ print toupper($1) }'`      | Uppercase me convert kare     |
| `tolower($1)`      | `awk '{ print tolower($1) }'`      | Lowercase me convert kare     |
| `substr($1,1,3)`   | `awk '{ print substr($1,1,3) }'`   | 1st column ke first 3 chars   |
| `index($1,"root")` | `awk '{ print index($1,"root") }'` | “root” ka position batata hai |

<br>

**Real-Life DevOps Examples**:

**Example 1: Disk Usage Report**:
```
df -h | awk 'NR>1 { print $1, $5 }'
```
Filesystem aur uska usage percentage print karega.

**Example 2: Log Filter (Error Count)**:
```
awk '/ERROR/ { count++ } END { print "Total Errors:", count }' app.log
```

**Example 3: CPU Usage Top Processes**:
```
ps aux | awk '$3 > 20 { print $1, $2, $3, $11 }'
```
Sirf un processes ko print karega jinka CPU usage 20% se jyada hai.

<br>

**awk Scripts (Long Programs)**:

Tum awk commands ko ek file me likh sakte ho, jaise:

script.awk
```
BEGIN { FS=","; print "Name | Score" }
{ if ($2 >= 80) print $1, "|", $2 }
END { print "----End----" }
```

Run:
```
awk -f script.awk marks.csv
```

<br>

**DevOps Automation Example**:

Suppose tumhare paas ek log file hai jisme columns hain:
```
service status time(ms)
nginx   200     134
api     500     289
db      200     156
```

Aur tumhe sirf failed (status != 200) services dikhani hain:
```
awk '$2 != 200 { print $1, "failed with code", $2 }' logs.txt
```
Output:
```
api failed with code 500
```
