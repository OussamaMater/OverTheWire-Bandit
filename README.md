# OverTheWire - Bandit
Hello! you'll find my solutions to the Bandit challenges with an explanation, hopefully, this helps you if you're struggling.
\
_This is a guide that helps you through the challenges, so you can follow up and actually do the challenges, so no passwords will be provided._
## Level 0
### Explanation
The goal of this challenge is to connect to the provided server via ssh.
\
To be able to use ssh you will always need 3 things:
1. The server's ip
2. The username
3. The password

so the command will be structured this way:
```
ssh username@ip
```
ssh uses 22 as a default port, but sometimes the administrator may change it, if that's the case, you'll have to provide it.
### Solution
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
# use bandit0 as a password as well
```
## Level 0 → 1
### Explanation
Now that you are inside the challenge server we can start solving these challenges.
\
Solving a challenge will give you the password of the next user, this way you can keep levelling up.
\
For this challenge, the password is stored in a file "readme" in the home directory of bandit0 user.
### Solution
```bash
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme 
[REDACTED]

```
## Level 1 → 2
### Explanation
This challenge is similar to the previous one, but with a twist.
\
The "cat" command is not always used to echo out files content, it has different usages, so for instance if no filename is provided or the filename is "-" it will read from the stdin instead.
\
[Read More - Article](https://linuxize.com/post/linux-cat-command/)
\
[Read More - Discussion](https://unix.stackexchange.com/questions/16357/usage-of-dash-in-place-of-a-filename)


### Solution
```bash
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ ./- # or use the full path
[REDACTED]

```
## Level 2 → 3
### Explanation
Yet, so similar to the previous challenges, you need to echo out the content of a file, but the filename is special, kind of, in our case it has spaces, so you can't provide it as an argument to the "cat" command, as it will be look for the files "spaces", "in", "this", "filename".
\
To solve this challenge we need to _escape_ the character space.
### Solution
```bash
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename
[REDACTED]

```
## Level 3 → 4
### Explanation
In this challenge we need to change directory and list _all_ the files.
\
In Linux and Unix systems, the files starting with . (a dot) are hidden files.
### Solution
```bash
bandit3@bandit:~$ ls -l
total 4
drwxr-xr-x 2 root root 4096 May  7  2020 inhere
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 May  7  2020 .
drwxr-xr-x 3 root    root    4096 May  7  2020 ..
-rw-r----- 1 bandit4 bandit3   33 May  7  2020 .hidden
bandit3@bandit:~$ cat .hidden
[REDACTED]

```
## Level 4 → 5
### Explanation
Same as the previous challenge we need to change the directory, but once we do it, we are granted 9 files, one of which contains the password.
\
The hint states that the file we are looking for is human-readable (text), so we need to perform a simple check against all the files using the "file" command.
### Solution
```bash
bandit4@bandit:~$ ls -l
total 4
drwxr-xr-x 2 root root 4096 May  7  2020 inhere
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ file ./* | grep "text" # there is no need for the grep in this level.
./-file07: ASCII text
bandit4@bandit:~/inhere$ cat ./-file07
[REDACTED]
```
## Level 5 → 6
### Explanation
In this challenge, we are granted 20 directories, each directory has multiple files, making it impossible to inspect the content of every file one by one, and only one out of these files has the password.
\
The file we are looking for has some unique properties:
1. human-readable
2. 1033 bytes in size
3. not executable

We can use the "find" command and get it easily.
### Solution
```bash
bandit5@bandit:~$ ls -l
total 4
drwxr-x--- 22 root bandit5 4096 May  7  2020 inhere
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ ls 
maybehere00  maybehere03  maybehere06  maybehere09  maybehere12  maybehere15  maybehere18
maybehere01  maybehere04  maybehere07  maybehere10  maybehere13  maybehere16  maybehere19
maybehere02  maybehere05  maybehere08  maybehere11  maybehere14  maybehere17
bandit5@bandit:~/inhere$ find . -readable -size 1033c ! -executable
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
[REDACTED]
```
## Level 6 → 7
### Explanation
Similar to the previous challenge, we need to look for a file, but this time, across the system, the file has these unique properties:
1. owned by user bandit7
2. owned by group bandit6
3. 33 bytes in size

The "find" command for the rescue!
### Solution
```bash
bandit6@bandit:~$ ls
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
[REDACTED]
```
## Level 7 → 8
### Explanation
This challenge requires us to filter a big text file by grabbing a specific word.
\
The "grep" command is suitable for the job.

### Solution
```bash
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ cat data.txt | grep "millionth"
millionth       [REDACTED]
bandit7@bandit:~$


```
## Level 8 → 9
### Explanation
Similar to the previous challenge, we need to filter a big text file, the line we are looking for is the only line that occurs once, so we need to remove all the duplicates and echo out the content.
### Solution
```bash
bandit8@bandit:~$ ls
data.txt
bandit8@bandit:~$ sort data.txt | uniq -u
[REDACTED]
```
## Level 9 → 10
### Explanation
Still the same style as the previous one, to get the password we need to do some kind of filtering.
\
In this challenge, the password we are looking for is in one of the few _human-readable_ strings, preceded by several = characters.
\
We will create our first very simple _regular expression_ (a pattern) to get all words preceded by at least one =.
\
Regular Expressions are so interesting and a great skill to have, here are some resources:
\
[Read More - Article 1](https://www.linux.com/topic/desktop/introduction-regular-expressions-new-linux-users/)
\
[Read More - Article 2](https://ubuntu.com/blog/regex-basics)
\
[Read More - Article 3](https://www.geeksforgeeks.org/how-to-use-regular-expressions-regex-on-linux)
\
[Read More - Video](https://www.youtube.com/watch?v=ZfQFUJhPqMM)

### Solution
```bash
bandit9@bandit:~$ ls
data.txt
bandit9@bandit:~$ strings data.txt | grep "=.*"
========== the*2i\"4
=:G e
========== password
<I=zsGi
Z)========== is
A=|t&E
Zdb=
c^ LAh=3G
*SF=s
&========== [REDACTED]
S=A.H&^
```
## Level 10 → 11
### Explanation
This time, there is no filtering, the challenge is straight forward, all we need to do is decoding the _base64_ text.
\
If you've never heard about this encoding, or what bases are, I highly recommend reading these articles:
\
[Read More - Article](https://code.tutsplus.com/tutorials/base-what-a-practical-introduction-to-base-encoding--net-27590)
\
[Read More - Article](https://www.base64encoder.io/learn/)

### Solution
```bash
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ cat data.txt | base64 -d
The password is [REDACTED]
```