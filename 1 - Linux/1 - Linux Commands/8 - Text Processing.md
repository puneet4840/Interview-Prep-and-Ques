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
