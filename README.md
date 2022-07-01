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