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
## Level 11 → 12
### Explanation
In this challenge the file content have been rotated by 13 positions, it's a common encryption called _rot13_ which replaces a letter with the 13th etter after it in the alphabet.
\
We can simply use the "tr" command and replace one set of alphabet with another set.
\
I highly recommend reading more about the tr command:
\
[Read More - Article](https://www.geeksforgeeks.org/tr-command-in-unix-linux-with-examples/)
\
And about the common substitution encryption methods:
\
[Read More - Article](http://practicalcryptography.com/ciphers/substitution-category/)

### Solution
```bash
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is [REDACTED]
```
## Level 12 → 13
### Explanation
This task is simple, but so repetitive, we have a Hex Dump that should be reverse engineered to a binary, and we start the decompression journey.
\
Note that in the solution I went quick, to know what decompression you are going for, use the "file" command as we learned in the previous challenges.
### Solution
```bash
bandit12@bandit:~$ mkdir /tmp/oussama
bandit12@bandit:~$ cd /tmp/oussama
bandit12@bandit:/tmp/oussama$ cp ~/data.txt .
bandit12@bandit:/tmp/oussama$ xxd -r data > binary
bandit12@bandit:/tmp/oussama$ mv binary binary.gz # we need that suffix for the command to work
bandit12@bandit:/tmp/oussama$ gzip -d binary.gz
bandit12@bandit:/tmp/oussama$ bzip2 -d binary
bandit12@bandit:/tmp/oussama$ mv binary.out binary.gz
bandit12@bandit:/tmp/oussama$ gzip -d binary.gz
bandit12@bandit:/tmp/oussama$ tar -xf binary
bandit12@bandit:/tmp/oussama$ tar -xf data5.bin
bandit12@bandit:/tmp/oussama$ bzip2 -d data6.bin 2>/dev/null
bandit12@bandit:/tmp/oussama$ tar -xf data6.bin.out
bandit12@bandit:/tmp/oussama$ mv data8.bin data8.gz
bandit12@bandit:/tmp/oussama$ gzip -d data8.gz
bandit12@bandit:/tmp/oussama$ cat data8
The password is [REDACTED]
```
## Level 13 → 14
### Explanation
Well this has to be one of the easiest tasks, as we are given the ssh private key.
\
All these challenges we've been using the _password_ to login, but this is only an option, as we can use the private key instead, and that's exactly what we've done here.
Read more about the two authentication methods:
\
[Read More - Article](https://www.appviewx.com/education-center/ssh-authentication-methods/)
### Solution
```bash
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost
```
## Level 14 → 15
### Explanation
To get the next password, we need to submit the current password to a service running on port 30000.
\
This can be done by using "netcat" command.
\
If you are planning on playing CTFs I highly recommend learning this command and how it works as it can be so handy.
\
[Read More - Article](https://linuxize.com/post/netcat-nc-command-with-examples/)
### Solution
```bash
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
[REDACTED - current password]
bandit14@bandit:~$ nc 127.0.0.1 30000
[REDACTED - current password] # simply paste the bandit14's password and hit enter
Correct!
[REDACTED - password for the next level]
```
## Level 15 → 16
### Explanation
It is almost the same challenge, the only difference is that server running this service is using SSL.
\
You can read more about SSL here:
\
[Read More - Article](https://www.websecurity.digicert.com/security-topics/what-is-ssl-tls-https)
### Solution
```bash
bandit15@bandit:~$ cat /etc/bandit_pass/bandit15
[REDACTED - current password]
bandit15@bandit:~$ ncat -C --ssl 127.0.0.1 30001
[REDACTED - current password]
Correct!
[REDACTED - password for the next level]
```
## Level 16 → 17
### Explanation
This challenge will test what we learned in the previous ones, we need to locate all the services running in a specific range, then connect to each one of them, submit the current password, and get a private key that will eventually help us get the wanted password.
### Solution
```bash
bandit16@bandit:~$ nmap -p 31000-32000 127.0.0.1
Starting Nmap 7.40 ( https://nmap.org ) at 2022-07-04 11:39 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00022s latency).
Not shown: 996 closed ports
PORT      STATE    SERVICE
31046/tcp open     unknown
31518/tcp filtered unknown
31691/tcp open     unknown
31790/tcp open     unknown
31960/tcp open     unknown

Nmap done: 1 IP address (1 host up) scanned in 1.25 seconds
bandit16@bandit:~$ cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect 127.0.0.1:31790
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
Correct!
-----BEGIN RSA PRIVATE KEY-----
[REDACTED]
-----END RSA PRIVATE KEY-----
bandit16@bandit:~$ mkdir /tmp/oussama
bandit16@bandit:~$ cd /tmp/oussama
bandit16@bandit:/tmp/oussama$ nano id_rsa # paste the content and hit ctrl+o then ctrl+x
bandit16@bandit:/tmp/oussama$ chmod 600 id_rsa
bandit16@bandit:/tmp/oussama$ ssh -i id_rsa bandit17@localhost
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
[REDACTED]
```
## Level 17 → 18
### Explanation
This challenge is straightforward, we have two files (passwords.old, passwords.new), the password for the next level is in passwords.new and is the only line that has been changed between the two files.
### Solution
```bash
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii
---
> [REDACTED]
```

## Level 18 → 19
### Explanation
In this challenge we are logged out as soon as we log in, the password is saved in a file called "readme" in the home directory, so we can simply echo out the content without connecting.
\
We are executing commands over ssh, this article can be useful:
\
[Read More - Article](https://www.cyberciti.biz/faq/unix-linux-execute-command-using-ssh/)
### Solution
```bash
oussama@oussama:~$ ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat ~/readme'
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.orgs password:
[REDACTED]
```
## Level 19 → 20
### Explanation
In this challenge, we will learn about the "setuid" (set user ID) which is a special type of file permission when set a user may execute the binary with a level of access that matches the user who owns the file.
\
[Read More - Article](https://www.computerhope.com/jargon/s/setuid.htm)
### Solution
```bash
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
[REDACTED]
```
## Level 20 → 21
### Explanation
In this challenge we are given a binary that has a setuid (like the previous challenge), this binary needs a port as an argument, it will connect to the service running on that port, reads the current password, if it matches, it will provide the next password.
\
So we need to create a service that when connected to, provides the current password, and run it in the background so continue using our session.
### Solution
```bash
bandit20@bandit:~$ ls
suconnect
bandit20@bandit:~$ ./suconnect
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
bandit20@bandit:~$ echo "[REDACTED - Current Password]" | netcat -lp 4444 &
[1] 15831
bandit20@bandit:~$ ./suconnect 4444
Read: [REDACTED - Current Password]
Password matches, sending next password
[REDACTED - New Password]
[1]+  Done echo "[REDACTED - Current Password]" | netcat -lp 4444
```