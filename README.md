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
## Level 21 → 22
### Explanation
In this challenge we will get to learn cronjobs, it's a scheduling utility present in unix-like systems.
\
We need to list the crons, check what scripts are being executed and based on that we can solve the challenge.
\
This one was straightforward, the cronjob will execute a script to copy the next level password to the "tmp" directory and we can simply echo out its content.
\
Knowing how cronjobs work and how to create/configure them is a must-have skill, so here are some resources:
\
[Read More - Article](https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/)
\
[Read More - Article](https://www.freecodecamp.org/news/cron-jobs-in-linux)
### Solution
```bash
bandit21@bandit:~$ cd /etc/cron.d
bandit21@bandit:/etc/cron.d$ ls
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:/etc/cron.d$ ls -l /usr/bin/cronjob_bandit22.sh # we can execute and read the script, so let's have a look
-rwxr-x--- 1 bandit22 bandit21 130 May  7  2020 /usr/bin/cronjob_bandit22.sh
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
[REDACTED]
```
## Level 22 → 23
### Explanation
This challenge is very similar to the previous challenge, but with a more complicated script that we need to understand.
\
The script is copying the password of whoever user executing it to the "tmp" directory in a file named the md5 hash of the name, if this makes sense.
\
So in short, the target is the md5 of the user executing the script.
### Solution
```bash
bandit22@bandit:~$ cd /etc/cron.d
bandit22@bandit:/etc/cron.d$ ls
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root
bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d   -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:/etc/cron.d$ echo I am user bandit23 | md5sum | cut -d   -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
[REDACTED]
```

## Level 23 → 24
### Explanation
Still with the cronjobs challenges, but this time we need to understand and write our first script.
\
Going through the script (code in the solution), we can see that it's simply deleting the files of a given directory, in our case "/var/spool/bandit24", but if the file is owned by the current user it will be executed, which means we need to create a script that copies the password to somewhere we can read it, like the "tmp" directory.
\
So here are the steps:
1. Create a script.
2. Make the script executable.
3. Create an empty file (where we will be saving our password).
4. Make the file _writable_ by "others", remember the script will be run as "bandit24", so the file permission should be something like xx2, xx3, xx6 or xx7.

You can read more about file permissions [here](https://www.guru99.com/file-permissions.html).
### Solution
```bash
bandit23@bandit:~$ cd /etc/cron.d
bandit23@bandit:/etc/cron.d$ ls
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i # the line we need to focus in
        fi
        rm -f ./$i
    fi
done
bandit23@bandit:/etc/cron.d$ mkdir /tmp/oussamalevel24
bandit23@bandit:/etc/cron.d$ cd /tmp/oussamalevel24
bandit23@bandit:/tmp/oussamalevel24$ nano script.sh
# the content of the script
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/oussamalevel24/bandit24
bandit23@bandit:/tmp/oussamalevel24$ chmod +x script.sh
bandit23@bandit:/tmp/oussamalevel24$ touch bandit24
bandit23@bandit:/tmp/oussamalevel24$ chmod a+w bandit24
bandit23@bandit:/tmp/oussamalevel24$ cp script.sh /var/spool/bandit24
bandit23@bandit:/tmp/oussamalevel24$ # wait one minute or so
bandit23@bandit:/tmp/oussamalevel24$ cat bandit24
[REDACTED]
```

## Level 24 → 25
### Explanation
In this challenge, we need to make another script to create 10000 combinations, so we can _brute-force_ the service running on port 30002.
\
The service accepts our current password plus a 4-digit pin separated by a space.
Example: bandit24_password 1234
\
Brute forcing is a very common attack that consists of an attacker submitting many passwords or passphrases with the hope of eventually guessing correctly, I highly recommend reading about its techniques and tools here:
\
[Read More - Article](https://resources.infosecinstitute.com/topic/kali-linux-top-5-tools-for-password-attacks/)
\
[Read More - Article](https://geekflare.com/brute-force-attack-tools/)
\
[Read More - Article](https://www.tutorialspoint.com/kali_linux/kali_linux_password_cracking_tools.htm)

### Solution
```bash
bandit24@bandit:~$ mkdir /tmp/oussamalevel25
bandit24@bandit:~$ cd /tmp/oussamalevel25
bandit24@bandit:/tmp/oussamalevel25$ nano script.sh
# paste the script content, we are simply creating all the possible combinations
#!/bin/bash

for i in {0000..9999}; do
        # note the use of >> so append and not overwrite
        echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ" $i >> passwords
done
bandit24@bandit:/tmp/oussamalevel25$ chmod +x script.sh
bandit24@bandit:/tmp/oussamalevel25$ ./script.sh
bandit24@bandit:/tmp/oussamalevel25$ cat passwords | nc localhost 30002 >> result
bandit24@bandit:/tmp/oussamalevel25$ cat result | grep 'The password'
The password of user bandit25 is [REDACTED]
```

## Level 25 → 26
### Explanation
In this challenge we will need to abuse the "more" command.
\
More command is used to view the text files in the command prompt, displaying one screen at a time in case the file is large, and the key to the challenge is that it will not finish executing as long as there is more content to be displayed.
\
Hopefully the solution will make sense.
### Solution
```bash
bandit25@bandit:~$ ls -l
total 4
-r-------- 1 bandit25 bandit25 1679 May  7  2020 bandit26.sshkey
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@127.0.0.1
# Connection closed immediately
bandit25@bandit:~$ cat /etc/passwd | grep "bandit26"
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext # not a regular shell
bandit25@bandit:~$ cat /usr/bin/showtext # let's have a look
#
#!/bin/sh

export TERM=linux

more ~/text.txt # we need to find a way not to hit the exit 0
exit 0
bandit25@bandit:~$ # minimise your terminal so there is always some content for "more" to display and this way we won't be exiting the script
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@127.0.0.1
bandit25@bandit:~$ # this time it won't exit, so let's abuse more, by entering vim, hit v
bandit25@bandit:~$ # we can use vim to spawn a shell, (make sure you are in command mode by hitting esc) type :set shell=/bin/bash then type :shell, that's it!
bandit26@bandit:~$ cat /etc/bandit_pass/bandit26
[REDACTED]
```
## Level 26 → 27
### Explanation
This challenge is pretty easy, we have a binary with that special file permission we discussed "setuid", so whenever executed it runs as the next level user, this way we can simply echo out the password.
### Solution
```bash
bandit26@bandit:~$ ls
bandit27-do  text.txt
bandit26@bandit:~$ ./bandit27-do
Run a command as another user.
  Example: ./bandit27-do id
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
[REDACTED]
```
## Level 27 → 28
### Explanation
In this challenge, we will practice our git skills, so we are given a remote repository that we need to clone and explore.
\
This level requires some knowledge of what and how "git" works, I highly recommend reading these two articles to get you started:
\
[Read More - Article](https://www.w3schools.com/git/)
\
[Read More - Article](https://www.atlassian.com/git)
### Solution
```bash
bandit27@bandit:~$ cd /tmp
bandit27@bandit:/tmp$ mkdir oussamalevel27
bandit27@bandit:/tmp$ cd oussamalevel27
bandit27@bandit:/tmp/oussamalevel27$ git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit27/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' cant be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit27/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit27-git@localhost password: # same password as bandit27
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
bandit27@bandit:/tmp/oussamalevel27$ ls
repo
bandit27@bandit:/tmp/oussamalevel27$ cd repo/
bandit27@bandit:/tmp/oussamalevel27/repo$ ls
README
bandit27@bandit:/tmp/oussamalevel27/repo$ cat README
The password to the next level is: [REDACTED]
```
## Level 28 → 29
### Explanation
Still practising our git skills, this time we need to go one step back to get the password.
\
This book will help you understand most of the git commands and the need for each one of them:
\
[Read More - Book](https://books.goalkicker.com/GitBook/)

### Solution
```bash
bandit28@bandit:~$ cd /tmp
bandit28@bandit:/tmp$ mkdir oussamalevel28
bandit28@bandit:/tmp$ cd oussamalevel28
bandit28@bandit:/tmp/oussamalevel28$ git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit28/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' cant be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit28/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit28-git@localhosts password:
remote: Counting objects: 9, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0)
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.
bandit28@bandit:/tmp/oussamalevel28$ cd repo/
bandit28@bandit:/tmp/oussamalevel28/repo$ ls
README.md
bandit28@bandit:/tmp/oussamalevel28/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx

bandit28@bandit:/tmp/oussamalevel28/repo$ git log
commit edd935d60906b33f0619605abd1689808ccdd5ee
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    fix info leak

commit c086d11a00c0648d095d04c089786efef5e01264
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    add missing data

commit de2ebe2d5fd1598cd547f4d56247e053be3fdc38
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    initial commit of README.md
bandit28@bandit:/tmp/oussamalevel28/repo$ git checkout c086d11a00c0648d095d04c089786efef5e01264
Note: checking out 'c086d11a00c0648d095d04c089786efef5e01264'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at c086d11... add missing data
bandit28@bandit:/tmp/oussamalevel28/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: [REDACTED]
```
## Level 29 → 30
### Explanation
Still with the git challenges, this time we will play around with branches.
\
Every concept is well explained in the book above (previous challenge), but still, if you do want a quick article here is
[one](https://www.atlassian.com/git/tutorials/using-branches) and here is an 
[interactive tutorial](https://learngitbranching.js.org/) as well.
### Solution
```bash
bandit29@bandit:~$ mkdir /tmp/oussamalevel29
bandit29@bandit:~$ cd /tmp/oussamalevel29
bandit29@bandit:/tmp/oussamalevel29$ git clone ssh://bandit29-git@localhost/home/bandit29-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit29/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' cant be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit29/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit29-git@localhosts password:
remote: Counting objects: 16, done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0)
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
bandit29@bandit:/tmp/oussamalevel29$ ls
README.md
bandit29@bandit:/tmp/oussamalevel29$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
bandit29@bandit:/tmp/oussamalevel29$ git branch -r # listing only the remote branches, you can use -a as well
  origin/HEAD -> origin/master
  origin/dev
  origin/master
  origin/sploits-dev
bandit29@bandit:/tmp/oussamalevel29$ git checkout dev
Branch dev set up to track remote branch dev from origin.
Switched to a new branch 'dev'
bandit29@bandit:/tmp/oussamalevel29$ ls
code  README.md
bandit29@bandit:/tmp/oussamalevel29$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: [REDACTED]
```
## Level 30 → 31
### Explanation
Well, we are still doing git challenges, and in this one, we will learn about "tags".
\
Sometimes, when a commit is an import change, we may want to bookmark it, give it a label/alias, just to mark that progress in code, for example name it "v1.0", to do so, we use tags.
\
In simple words, tags are labels that can be used to identify a specific commit.
\
I highly recommend reading more about tags:
\
[Read More - Article](https://linuxhint.com/use-git-tags/)
\
[Read More - Article](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag)
\
[Read More - Article](https://www.freecodecamp.org/news/git-tag-explained-how-to-add-remove/)
\
And the book I shared in the previous challenges.
### Solution
```bash
bandit29@bandit:~$ mkdir /tmp/oussamalevel30 && cd $_
bandit30@bandit:/tmp/oussamalevel30$ git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit30/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' cant be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit30/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit30-git@localhosts password:
remote: Counting objects: 4, done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (4/4), done.
bandit30@bandit:/tmp/oussamalevel30$ cd repo/
bandit30@bandit:/tmp/oussamalevel30/repo$ git tag
secret
bandit30@bandit:/tmp/oussamalevel30/repo$ git show secret
[REDACTED]
```
## Level 31 → 32
### Explanation
This is the last "git" challenge, we need to push a txt file with a specific name and content.
\
But, there is a ".gitignore" file that ignores all txt files, so we need to get rid of it.
\
This file is really useful in our projects, so maybe read this article:
\
[Read More - Article](https://www.pluralsight.com/guides/how-to-use-gitignore-file)

### Solution
```bash
bandit31@bandit:~$ mkdir /tmp/oussamalevel31 && cd clear
bandit31@bandit:/tmp/oussamalevel31$ git clone ssh://bandit31-git@localhost/home/bandit31-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit31/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' cant be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit31-git@localhosts password:
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (4/4), 383 bytes | 0 bytes/s, done.
bandit31@bandit:/tmp/oussamalevel31$ cd repo/
bandit31@bandit:/tmp/oussamalevel31/repo$ ls
README.md
bandit31@bandit:/tmp/oussamalevel31/repo$ cat README.md
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master

bandit31@bandit:/tmp/oussamalevel31/repo$ cat .gitignore
*.txt
bandit31@bandit:/tmp/oussamalevel31/repo$ rm .gitignore
bandit31@bandit:/tmp/oussamalevel31/repo$ echo 'May I come in?' > key.txt
bandit31@bandit:/tmp/oussamalevel31/repo$ git add .
bandit31@bandit:/tmp/oussamalevel31/repo$ git commit -m 'Added key.txt'
[master f312353] Added key.txt
 2 files changed, 1 insertion(+), 1 deletion(-)
 delete mode 100644 .gitignore
 create mode 100644 key.txt
bandit31@bandit:/tmp/oussamalevel31/repo$ git push origin master
Could not create directory '/home/bandit31/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' cant be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit31-git@localhosts password:
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 289 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password for the next level:
remote: [56a9bf19c63d650ce78e6ec0354ee45e]
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
To ssh://localhost/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://bandit31-git@localhost/home/bandit31-git/repo'
```
## Level 32 → 33
### Explanation
This is the last challenge, where we need to escape a special shell that executes any given command as uppercase, and as you know a binary's name is case sensitive, so "ls" is different from "LS".
\
Well, the idea here is to use something that won't be affected by this conversion.
\
After some Googling, I found out that there is a special environment variable that stores the path to the shell so it stores "/bin/sh" or "/bin/bash" depending on shell being used, so the idea is to pass the "$0" and hope it re-executes the shell.
\
Some helpful resources:
\
[Read More - Discussion](https://unix.stackexchange.com/questions/280454/what-is-the-meaning-of-0-in-the-bash-shell)
\
[Read More - Article](https://bash.cyberciti.biz/guide/$0)
### Solution
```bash
WELCOME TO THE UPPERCASE SHELL
>> ls
sh: 1: LS: not found
>> $0
$ ls -l
total 8
-rwsr-x--- 1 bandit33 bandit32 7556 May  7  2020 uppershell
$ whoami
bandit33
```