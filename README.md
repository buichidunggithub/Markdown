# MANAGE SERVER WITH LINUX
## Table of contents:
- Introduction
	* [What is kernel ?](#kernel)
	* [What is Shell ?](#shell)
	* [Command Line Shell](#command)
	* [Graphical Shells](#graphical)
	* [Shell Scripting](#script)
	* [Why do we need shell scripts](#need)
	* [Advantages of shell scripts](#advantages)
	* [Disadvantages of shell scripts](#disadvantages)
	* [Simple demo of shell scripting using Bash Shell](#demo)
- Role Definition
	* [ls](#ls)
	* [stat](#stat)
	* [useradd](#useradd)
	* [passwd](#passwd)
	* [usermod](#usermod)
- [Processes Management](#processes-management)
- System Resources
	* [top](#top)
	* [vmstat](#vmstat)
	* [lsof](#lsof)
	* [tcpdump](#tcpdump)
	* [netstat](#netstat)
	* [htop](#htop)
	* [iotop](#iotop)
	* [iostat](#iostat)
	* [iptraf-ng](#iptraf-ng)
	* [iftop](#iftop)
- [Installation Packages Management](#installation-packages-management)
- [Log Server Management](#log-server-management)
- [Console](#console)
- [Network and IPTables](#network-and-iptables)
- [Web Server Installation](#web-server-installation)
- [Services and Network in Host](#services-and-network-in-host)
- [Other skills](#other-skills)
- [References](#references)

***

## *Introduction* <a name="intro"></a>

If you are using any major operating system you are indirectly interacting to shell. If you are running Ubuntu, Linux Mint or any other Linux distribution, you are interacting to shell every time you use terminal. In this article I will discuss about linux shells and shell scripting so before understanding shell scripting we have to get familiar with following terminologies:

* Kernel
* Shell
* Terminal


### What is Kernel ? <a name="kernel"></a>

The kernel is a computer program that is the core of a computer's operating system, with complete control over everthing in the system. It manages following resources of the Linux system - 

* File management
* Process management
* I/O management
* Memory management
* Device management etc.

### What is Shell ? <a name="shell"></a>

A shell is special user program which provide  an interface to user to use operating system services. Shell accept human readble commands from user and convert them into something which kernel can such as keyboards or from files. The shell gets started when the user logs in or start the terminal.

![linux shell](https://media.geeksforgeeks.org/wp-content/uploads/18834419_1198504446945937_35839918_n-300x291.png)

Shell is broadly classified into two categories -

* Command Line Shell
* Graphical Shell

### Command Line Shell <a name="command"></a>

Shell can be accessed by user using a command line interface. A special program called Terminal in linux/macOS or Command Prompt in Windows OS is provided to type in the human readable commands such as “cat”, “ls” etc. and then it is being execute. The result is then displayed on the terminal to the user. A terminal in Ubuntu 16.4 system looks like this –

![linux command line](https://media.geeksforgeeks.org/wp-content/uploads/cli_example.png)

In above screenshot “ls” command with “-l” option is executed.
It will list all the files in current working directory in long listing format.
Working with command line shell is bit difficult for the beginners because it’s hard to memorize so many commands. It is very powerful, it allows user to store commands in a file and execute them together. This way any repetitive task can be easily automated. These files are usually called batch files in Windows and Shell Scripts in Linux/macOS systems.

### Graphical Shells <a name="graphical"></a>

Graphical shells provide means for manipulating programs based on graphical user interface (GUI), by allowing for operations such as opening, closing, moving and resizing windows, as well as switching focus between windows. Window OS or Ubuntu OS can be considered as good example which provide GUI to user for interacting with program. User do not need to type in command for every actions.A typical GUI in Ubuntu system –

[GUI shell](https://media.geeksforgeeks.org/wp-content/uploads/GUI-shell.png)

There are several shells are available for Linux systems like –

* BASH (Bourne Again SHell) – It is most widely used shell in Linux systems. It is used as default login shell in Linux systems and in macOS. It can also be installed on Windows OS.
* CSH (C SHell) – The C shell’s syntax and usage are very similar to the C programming language.
* KSH (Korn SHell) – The Korn Shell also was the base for the POSIX Shell standard specifications etc.

Each shell does the same job but understand different commands and provide different built in functions.

### Shell scripting <a name="script"></a>

  Usually shells are interactive that mean, they accept command as input from users and execute them. However some time we want to execute a bunch of commands routinely, so we have type in all commands each time in terminal.
As shell can also take commands as input from file we can write these commands in a file and can execute them in shell to avoid this repetitive work. These files are called Shell Scripts or Shell Programs. Shell scripts are similar to the batch file in MS-DOS. Each shell script is saved with .sh file extension eg. myscript.sh
  A shell script have syntax just like any other programming language. If you have any prior experience with any programming language like Python, C/C++ etc. it would be very easy to get started with it.
A shell script comprises following elements –

* Shell Keywords – if, else, break etc.
* Shell commands – cd, ls, echo, pwd, touch etc.
* Functions
* Control flow – if..then..else, case and shell loops etc.

### Why do we need shell scripts <a name="need"></a>

There are many reasons to write shell scripts –

* To avoid repetitive work and automation
* System admins use shell scripting for routine backups
* System monitoring
* Adding new functionality to the shell etc.

### Advantages of shell scripts <a name="advantages"></a>

* The command and syntax are exactly the same as those directly entered in command line, so programmer do not need to switch to entirely different syntax
* Writing shell scripts are much quicker
* Quick start
* Interactive debugging etc.

### Disadvantages of shell scripts <a name="disadvantages"></a>

* Prone to costly errors, a single mistake can change the command which might be harmful
* Slow execution speed
* Design flaws within the language syntax or implementation
* Not well suited for large and complex task
* Provide minimal data structure unlike other scripting languages. etc

### Simple demo of shell scripting using Bash Shell <a name="demo"></a>

If you work on terminal, something you traverse deep down in directories. Then for coming few directories up in path we have to execute command like this as shown below to get the "python" directory -

[](https://media.geeksforgeeks.org/wp-content/uploads/old_way_cd.png)

It is quite frustrating, so why not we can have a utility where we just have to type the name of directory and we can directly jump to that without executing “cd ../” command again and again. Save the script as “jump.sh”

    # !/bin/bash 

    # A simple bash script to move up to desired directory level directly 

    function jump() 
    { 
      # original value of Internal Field Separator 
      OLDIFS=$IFS 

      # setting field separator to "/"  
      IFS=/ 

      # converting working path into array of directories in path 
      # eg. /my/path/is/like/this 
      # into [, my, path, is, like, this] 
      path_arr=($PWD) 

      # setting IFS to original value 
      IFS=$OLDIFS 

      local pos=-1 

      # ${path_arr[@]} gives all the values in path_arr 
      for dir in "${path_arr[@]}"
      do
          # find the number of directories to move up to 
          # reach at target directory  
          pos=$[$pos+1] 
          if [ "$1" = "$dir" ];then

              # length of the path_arr 
              dir_in_path=${#path_arr[@]} 

              #current working directory 
              cwd=$PWD 
              limit=$[$dir_in_path-$pos-1] 
              for ((i=0; i<limit; i++)) 
              do
                  cwd=$cwd/.. 
              done
              cd $cwd 
              break
          fi
      done
    }

For now we cannot execute our shell script because it do not have permissions. We have to make it executable by typing following command -

    $ chmod -x path/to/our/file/jump.sh

Now to make this available on every terminal session, we have to put this in ".bashrc" file. “.bashrc” is a shell script that Bash shell runs whenever it is started interactively. The purpose of a .bashrc file is to provide a place where you can set up variables, functions and aliases, define our prompt and define other settings that we want to use whenever we open a new terminal window.
Now open terminal and type following command –

    $ echo “source ~/path/to/our/file/jump.sh”>> ~/.bashrc

Now open  your terminal and try out new "jump" functionality by typing following command -

    $ jump dir_name

just like below screenshot -

> **Resources for learning Bash Scripting** 

* https://bash.cyberciti.biz/guide/The_bash_shell
* http://tldp.org/LDP/abs/html/

> **References**

* https://en.wikipedia.org/wiki/Shell_script
* https://en.wikipedia.org/wiki/Shell_(computing)

This article is contributed by Atul Kumar. If you like GeeksforGeeks and would like to contribute, you can also write an article using contribute.geeksforgeeks.org or mail your article to contribute@geeksforgeeks.org. See your article appearing on the GeeksforGeeks main page and help other Geeks.Please write comments if you find anything incorrect, or you want to share more information about the topic discussed above.

Attention reader! Don’t stop learning now. Get hold of all the important CS Theory concepts for SDE interviews with the CS Theory Course at a student-friendly price and become industry ready.

[](https://media.geeksforgeeks.org/wp-content/uploads/jump_way_cd-1.png)

> REFERENCE:  https://www.geeksforgeeks.org/introduction-linux-shell-shell-scripting/

***

## *Role Definition*
 
### `ls`

> `ls` is one of the basic commands that any Linux user should know.
>
> The `ls` command lists files and directories within the file system, and shows detailed information about them. It is a part of the GNU core utilities package which `is` installed on all Linux distributions.

**1. List files using ls with no option**

**ls** with no option list files and directories in bare format where we won’t be able to view details like file types, size, modified date and time, permission and links etc.
```
buichidung@CPU002169:~/Documents/Studying$ ls
awesome-python  Git  gittemp  Project_temp  root  Studying
```
**2. List files with option -l**

Here, **ls -l** (-l is character not one) shows file or directory, size, modified date and time, file or folder name and owner of file and its permission.

```
buichidung@CPU002169:~/Documents/Studying$ ls -l
total 20
drwxrwxr-x 5 buichidung buichidung 4096 Thg 10 19 11:24 awesome-python
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 14:14 Git
-rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 07:32 Project_temp
drwxrwxr-x 4 buichidung buichidung 4096 Thg 10 20 14:57 root
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 19 11:27 Studying
```

**3. View hidden files**

List all files including hidden file starting with '.'.

```
buichidung@CPU002169:~/Documents/Studying$ ls -a
.  ..  awesome-python  .git  Git  gittemp  Project_temp  root  Studying
```

**4. List files with Human Readable Format with option -lh**

With combination of **-lh** option, shows sizes in human readable format.

```
buichidung@CPU002169:~/Documents/Studying$ ls -lh
total 20K
drwxrwxr-x 5 buichidung buichidung 4,0K Thg 10 19 11:24 awesome-python
drwxrwxr-x 3 buichidung buichidung 4,0K Thg 10 20 14:14 Git
-rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
drwxrwxr-x 3 buichidung buichidung 4,0K Thg 10 20 07:32 Project_temp
drwxrwxr-x 4 buichidung buichidung 4,0K Thg 10 20 14:57 root
drwxrwxr-x 3 buichidung buichidung 4,0K Thg 10 19 11:27 Studying
```

**5. List Files and Directories with '/' character at the end**

Using **-F** option with **ls** command, will add the '/' character at the end each directory

```
buichidung@CPU002169:~/Documents/Studying$ ls -F
awesome-python/  Git/  gittemp  Project_temp/  root/  Studying/
```

**6. List files in reverse order**

The following command with **ls** -r option display files and irectories in reverse order

```
buichidung@CPU002169:~/Documents/Studying$ ls -r
Studying  root  Project_temp  gittemp  Git  awesome-python
```

**7. Recursively list Sub-Directories**

**ls -R** option will list very long listing directory trees. See an example of output of the command.

```
buichidung@CPU002169:~/Documents/Studying$ ls -R
.:
awesome-python  Git  gittemp  Project_temp  root  Studying

./awesome-python:
CONTRIBUTING.md  LICENSE   mkdocs.yml  requirements.txt
docs             Makefile  README.md   sort.py

./awesome-python/docs:
CNAME  css

./awesome-python/docs/css:
extra.css

./Git:
main.py

./Project_temp:
main.py

./root:
constant.py  database.pyc  __pycache__  temp.txt
database.py  main.py       student.py   util.py

./root/__pycache__:
constant.cpython-38.pyc  student.cpython-38.pyc
database.cpython-38.pyc  util.cpython-38.pyc

./Studying:
```

**8. Reverse output order**

With combination of **-ltr** will shows lastest modification ifle or directory date as last

```
buichidung@CPU002169:~/Documents/Studying$ ls -ltr
total 20
-rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
drwxrwxr-x 5 buichidung buichidung 4096 Thg 10 19 11:24 awesome-python
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 19 11:27 Studying
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 07:32 Project_temp
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 14:14 Git
drwxrwxr-x 4 buichidung buichidung 4096 Thg 10 20 14:57 root
```

**9. Sort file by file size**

With combination of -lS displays file size in order, will display big in size first

```
buichidung@CPU002169:~/Documents/Studying$ ls -lS
total 20
drwxrwxr-x 5 buichidung buichidung 4096 Thg 10 19 11:24 awesome-python
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 14:14 Git
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 07:32 Project_temp
drwxrwxr-x 4 buichidung buichidung 4096 Thg 10 20 14:57 root
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 19 11:27 Studying
-rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
```

**10. Display inot number of File or Directory**

We can see some number printed before file / directory name. With **-i** options list file / directory with inode number

```
buichidung@CPU002169:~/Documents/Studying$ ls -i
39980431 awesome-python  39980427 gittemp       39980399 root
39980378 Git             43123170 Project_temp  39980504 Studying
```

**11. Shows version of ls command**

Check version of ls command

```
buichidung@CPU002169:~/Documents/Studying$ ls --version
ls (GNU coreutils) 8.30
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by Richard M. Stallman and David MacKenzie.
```

**12. Show help page**

List help page of ls command with their option.

```
buichidung@CPU002169:~/Documents/Studying$ ls --version
...
```

**13. List Directory Information**

With **ls -l** command list files under directory **/tmp**. Wherein with **-ld** parameters displays information of **/tmp** directory.

```
buichidung@CPU002169:~/Documents/Studying$ ls -l /tmp
total 56
-rw------- 1 buichidung buichidung    0 Thg 3  22 07:05 config-err-fsKJ2L
drwxr-xr-x 2 buichidung buichidung 4096 Thg 3  22 07:16 hsperfdata_buichidung
drwx------ 3 root       root       4096 Thg 3  22 07:05 snap.snap-store
drwx------ 2 buichidung buichidung 4096 Thg 3  22 07:05 ssh-qJOOdh0gGdVG
drwx------ 3 root       root       4096 Thg 3  22 06:51 systemd-private-1ab5795ef2be417781758e590d164531-colord.service-tzoq1f
drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-ModemManager.service-F6wNIh
drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-switcheroo-control.service-BNEE8g
drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-systemd-logind.service-nvGLwh
drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-systemd-resolved.service-dpgimj
drwx------ 3 root       root       4096 Thg 3  22 06:50 systemd-private-1ab5795ef2be417781758e590d164531-systemd-timesyncd.service-kDUO6e
drwx------ 3 root       root       4096 Thg 3  22 06:51 systemd-private-1ab5795ef2be417781758e590d164531-upower.service-AhPEhg
-rw------- 1 buichidung buichidung    0 Thg 3  22 07:45 tpxUkxyz
drwx------ 2 buichidung buichidung 4096 Thg 3  22 09:18 tracker-extract-files.1001
drwx------ 2 gdm        gdm        4096 Thg 3  22 06:51 tracker-extract-files.125
drwx------ 2 postgres   postgres   4096 Thg 3  22 07:16 tracker-extract-files.127
drwx------ 4 buichidung buichidung 4096 Thg 3  22 07:45 vmware-buichidung
```

```
buichidung@CPU002169:~/Documents/Studying$ ls -ld /tmp/
drwxrwxrwt 22 root root 4096 Thg 3  22 13:48 /tmp/
```

**14. Display UID and GID of Files**

To display **UID** and **GID** of files and directories. Use option -n with ls command

```
buichidung@CPU002169:~/Documents/Studying$ ls -n
total 20
drwxrwxr-x 5 1001 1001 4096 Thg 10 19 11:24 awesome-python
drwxrwxr-x 3 1001 1001 4096 Thg 10 20 14:14 Git
-rw-rw-r-- 1 1001 1001    0 Thg 10 19 11:17 gittemp
drwxrwxr-x 3 1001 1001 4096 Thg 10 20 07:32 Project_temp
drwxrwxr-x 4 1001 1001 4096 Thg 10 20 14:57 root
drwxrwxr-x 3 1001 1001 4096 Thg 10 19 11:27 Studying
```

**15. ls command and its Aliases**

We have made alias for **ls** command, when we execute ls command it will take **-l** option by default and display long listing as mentioned earlier

```
buichidung@CPU002169:~/Documents/Studying$ alias ls="ls -l"
buichidung@CPU002169:~/Documents/Studying$ ls
total 20
drwxrwxr-x 5 buichidung buichidung 4096 Thg 10 19 11:24 awesome-python
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 14:14 Git
-rw-rw-r-- 1 buichidung buichidung    0 Thg 10 19 11:17 gittemp
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 20 07:32 Project_temp
drwxrwxr-x 4 buichidung buichidung 4096 Thg 10 20 14:57 root
drwxrwxr-x 3 buichidung buichidung 4096 Thg 10 19 11:27 Studying
```

To remove an alias previously defined, just use the unalias command

```
buichidung@CPU002169:~/Documents/Studying$ unalias ls
buichidung@CPU002169:~/Documents/Studying$ ls
awesome-python	Git  gittemp  Project_temp  root  Studying
buichidung@CPU002169:~/Documents/Studying$
```
	
Reference Link: https://www.tecmint.com/15-basic-ls-command-examples-in-linux/

### `stat`

> `stat` is a command-line utility that displays detailed information about given files or file systems.

**1. Check linux file status**

The easiest way to use **stat** is to provide it a file as an argument. The following command will display the size, blocks, IO blocks, file type, inode value, number of links and much more information about the file /var/log/syslog:

```
buichidung@CPU002169:/$ stat /var/log/syslog
File: /var/log/syslog
Size: 337544    	Blocks: 672        IO Block: 4096   regular file
Device: 802h/2050d	Inode: 56623998    Links: 1
Access: (0640/-rw-r-----)  Uid: (  104/  syslog)   Gid: (    4/     adm)
Access: 2021-03-22 06:50:48.397810754 +0700
Modify: 2021-03-22 14:24:21.420299006 +0700
Change: 2021-03-22 14:24:21.420299006 +0700
Birth: -
```

**2. Check file system status**
	
In the previous example, stat command treated the input file as a normal file, however, to display file system status instead of file status, use the `-f` option.

```
buichidung@CPU002169:/$ stat -f /var/log/syslog
File: "/var/log/syslog"
  ID: 64f6f9422fad26f Namelen: 255     Type: ext2/ext3
Block size: 4096       Fundamental block size: 4096
Blocks: Total: 239965964  Free: 225936043  Available: 213728991
Inodes: Total: 61022208   Free: 60163401
```

```
buichidung@CPU002169:/$ stat -f /
File: "/"
  ID: 64f6f9422fad26f Namelen: 255     Type: ext2/ext3
Block size: 4096       Fundamental block size: 4096
Blocks: Total: 239965964  Free: 225935968  Available: 213728916
Inodes: Total: 61022208   Free: 60163401
```

**3. Enable following of symbolic links**

Since Linux supports links  (**symbolic** and **hard links**), certain files may have one or more links, or they could even exist in a filesystem.

To enable stat to follow links, use the `-L` flag as shown

```
buichidung@CPU002169:/$ stat -L /
File: /
Size: 4096      	Blocks: 8          IO Block: 4096   directory
Device: 802h/2050d	Inode: 2           Links: 21
Access: (0755/drwxr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2021-03-22 06:50:30.781811119 +0700
Modify: 2020-12-08 10:30:23.302144007 +0700
Change: 2020-12-08 10:30:23.302144007 +0700
Birth: -
```

**4. Use a custom format to display information**

`stat` also allows you to use a particular or custom format instead of the default. The `-c` flag is used to specify the format used, it prints a newline after each use of format sequence.

Alternatively, you can use the `--printf` option which enables interpreting of backslash escapes sequences and turns off printing of a trailing newline. You need to use `\n` in the format to print a new line, for example.

```
stat --printf='%U\n%G\n%C\n%z\n' /var/log/secure
```

Meaning of the format sequences for files used in above example:

- **%U** - user name of owner
- **%G** - group name of owner
- **%C** - SELinux security context string
- **%z** - time of last status change, human-readable

```
stat --printf='%n\n%a\n%b\n' /
```

Meaning of the format sequences used in the above command
- **%n** - shows the file name
- **%a** - print free blocks available to non-superuser
- **%b** - outputs total data blocks in file system

**5. Print Information in Terse Form**

The `-t` option can be used to print the information in terse form.

```
buichidung@CPU002169:/$ stat -t /var/log/syslog
/var/log/syslog 338564 672 81a0 104 4 802 56623998 1 0 0 1616370648 1616398501 1616398501 0 4096
```

As a last note, your shell may have its own version of stat, please refer to your shell's documentation for details about the options it supports. To see all accepted output format sequences, refer to the stat man page.

```
man stat
```
	
Reference link: https://www.tecmint.com/linux-stat-command-examples/ 

### `grep`

> The grep filter searches a file for a particular pattern of characters, and displays all lines that contain that pattern. The pattern that is searched in the file is referred to as the regular expression (grep stands for globally search for regular expression and print out).

Syntax:

	```
	grep [options] pattern [files]
	```

Options description:

	```
	Options Description
	-c : This prints only a count of the lines that match a pattern
	-h : Display the matched lines, but do not display the filenames.
	-i : Ignores, case for matching
	-l : Displays list of a filenames only.
	-n : Display the matched lines and their line numbers.
	-v : This prints out all the lines that do not matches the pattern
	-e exp : Specifies expression with this option. Can use multiple times.
	-f file : Takes patterns from file, one per line.
	-E : Treats pattern as an extended regular expression (ERE)
	-w : Match whole word
	-o : Print only the matched parts of a matching line,
	with each such part on a separate output line.

	-A n : Prints searched line and nlines after the result.
	-B n : Prints searched line and n line before the result.
	-C n : Prints searched line and n lines after before the result.
	```

Create a file with content following:

	```
	buichidung@CPU002169:~/Documents/Studying$ nano hello.txt
	```

	```
	unix is great os. unix is opensource. unix is free os.
	Unix linux which one you choose.
	uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
	```

**1. Case insensitive search** 

The -i option enables to search for a string case insensitively in the give file. It matches the words like "UNIX", "Unix", "unix".

```
buichidung@CPU002169:~/Documents/Studying$ grep -i "UNIx" hello.txt
unix is great os. unix is opensource. unix is free os.
Unix linux which one you choose.
uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
```

**2. Displaying the count of number of matches**

We can find the number of liens that matches the given string/pattern

```
buichidung@CPU002169:~/Documents/Studying$ grep -c "unix" hello.txt 
2
```

**3. Display the file names that matches the pattern**

We can just display the files that contains the given string/pattern

```
buichidung@CPU002169:~/Documents/Studying$ grep -l "unix" *
grep: awesome-python: Is a directory
grep: Git: Is a directory
hello.txt
grep: Project_temp: Is a directory
grep: root: Is a directory
grep: Studying: Is a directory
```

or

```
buichidung@CPU002169:~/Documents/Studying$ grep -l "unix" hello.txt hello1.txt
hello.txt

```

**4. Checking for the whole words in a file** 

By default, grep matches the given string/pattern even if it  found as a substring in a file. The -w option to grep makes it match only the whole words.

```
buichidung@CPU002169:~/Documents/Studying$ grep -w "unix" hello.txt
unix is great os. unix is opensource. unix is free os.
uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
```  

**5. Displaying only the matched pattern**

By default display the entire line which has the matched string. We can make the grep to display only the matched string by using the -o option.

```
buichidung@CPU002169:~/Documents/Studying$ grep -o "unix" hello.txt
unix
unix
unix
unix
unix
unix
```

**6. Show line number while displaying the output using grep -n**

To show the line number of file with the line matched

```
buichidung@CPU002169:~/Documents/Studying$ grep -n "unix" hello.txt
1:unix is great os. unix is opensource. unix is free os.
4:uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
```

**7. Inverting the pattern match** 

You can display the lines that are not matchd with the specified search sting pattern using the -v option.

```
buichidung@CPU002169:~/Documents/Studying$ grep -v "unix" hello.txt
learn operating system.
Unix linux which one you choose.
```

**8. Matching the lines that start with a string**

The ^ regular expression pattern specifies the start of a line. This can be used in grep to match the lines which start with the given string or pattern.

```
buichidung@CPU002169:~/Documents/Studying$ grep "^unix" hello.txt
unix is great os. unix is opensource. unix is free os.
```

**9. Matching the lines that end with a string** 

The $ regular expression pattern specifies the end of a line. This can be used in grep to match the lines which end with the given string or pattern.

```
buichidung@CPU002169:~/Documents/Studying$ grep "oose.$" hello.txt
Unix linux which one you choose.
```

**10. Specifies expression with -e option** 

Can use multiple times:

```
buichidung@CPU002169:~/Documents/Studying$ $grep –e "Agarwal" –e "Aggarwal" –e "Agrawal" geekfile.txt
```

**11. -f file option Takes patterns from file, one per line**

Example:
```
buichidung@CPU002169:~/Documents/Studying$ grep -f hello.txt hello1.txt
```

**12. Print n specific lines from a file**

-A print the searched line and n lines after the result, -B prints the searched line and n lines before the result, and -C prints the searched line and n lines after and before the result.

Syntax:

```
$grep -A[NumberOfLines(n)] [search] [file]  
$grep -B[NumberOfLines(n)] [search] [file]  
$grep -C[NumberOfLines(n)] [search] [file] 
```

```
buichidung@CPU002169:~/Documents/Studying$ grep -A1 learn hello.txt
learn operating system.
Unix linux which one you choose.
uNix is easy to learn.unix is a multiuser os.Learn unix .unix is a powerful.
```

Reference Link: https://www.geeksforgeeks.org/grep-command-in-unixlinux/

### `useradd`

### `passwd`

### `usermod`

***

## *Processes Management*
<details>
<summary>Quản lý Processes</summary>
</details>

***

## *System Resources*

### `top`

> This is one of the most frequently used commands in our daily system administrative jobs. This command displays processor activity of Linux box and also displays tasks managed by kernel in real-time.

**1. Display of top command**
        
```
buichidung@CPU002169:~$ top

top - 09:04:38 up 47 min,  1 user,  load average: 1,02, 1,10, 1,29
Tasks: 288 total,   1 running, 287 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5,6 us,  1,4 sy,  0,0 ni, 91,7 id,  1,3 wa,  0,0 hi,  0,0 si,  0,0 st
MiB Mem :  15897,0 total,   7075,1 free,   4126,0 used,   4695,9 buff/cache
MiB Swap:   2048,0 total,   2048,0 free,      0,0 used.  10456,6 avail Mem 

PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                             
4059 buichid+  20   0 4785316 250288 122936 S   6,0   1,5   2:02.43 chrome                                                              
6988 buichid+  20   0 8943396 180040 100504 S   5,6   1,1   2:49.88 chrome                                                              
8862 buichid+  20   0 4708804 234228 113488 S   4,0   1,4   0:31.35 chrome                                                              
3802 buichid+  20   0  897336 204420 120932 S   2,7   1,3   4:14.21 chrome                                                              
2602 buichid+  20   0 1126968 287424 150416 S   2,3   1,8   2:38.23 chrome                                                              
2020 buichid+  20   0  870844  77424  45464 S   1,7   0,5   1:49.17 Xorg                                                                
4665 buichid+  20   0 4882464 798568  65872 S   1,3   4,9   2:07.56 java                                                                
7386 buichid+  20   0 1976064 252356 112628 S   1,3   1,6   0:53.05 Telegram                                                            
8921 buichid+  20   0 4658900 176296 126212 S   1,3   1,1   0:07.88 chrome                                                              
10003 buichid+  20   0  966176  52920  39408 S   1,0   0,3   0:01.61 gnome-terminal-                                                     
2174 buichid+  20   0 4531392 286724 111584 S   0,7   1,8   2:41.89 gnome-shell                                                         
670 avahi     20   0    8848   4012   3516 S   0,3   0,0   0:03.30 avahi-daemon                                                        
1148 mysql     20   0 2199720 361572  34840 S   0,3   2,2   0:06.66 mysqld                                                              
3810 buichid+  20   0  394324 115224  67216 S   0,3   0,7   0:29.61 chrome                                                              
6999 buichid+  20   0 9043004 338348 116476 S   0,3   2,1   0:38.68 chrome                                                              
10053 buichid+  20   0   14868   4312   3528 R   0,3   0,0   0:00.06 top                                                                 
1 root      20   0  169264  13168   8388 S   0,0   0,1   0:02.26 systemd                                                             
2 root      20   0       0      0      0 S   0,0   0,0   0:00.00 kthreadd                                                            
3 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_gp                                                              
4 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_par_gp                                                          
6 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 kworker/0:0H-kblockd                                                
9 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 mm_percpu_wq                                                        
10 root      20   0       0      0      0 S   0,0   0,0   0:00.04 ksoftirqd/0 
```
As we can see, tge default display contains two areas of information: the summary area (dashboard) and the task area (process list). By default, top will be updated after 3 seconds.

The first line of numbers on the dashboard includes time, how long our computer has been running, the number of people logged in, and what the load average has been for the past one, five, and 15 minutes. The second line shows the number of tasks and their states: running, stopped, sleeping, or zombie.

The third line displays the following central processing unit (CPU) values:

- `us`: Amount of time the CPU spends executing processes for people in “user space.”
- `sy`: Amount of time spent running system “kernel space” processes.
- `ni`: Amount of time spent executing processes with a manually set nice value.
- `id`: Amount of CPU idle time.
- `wa`: Amount of time the CPU spends waiting for I/O to complete.
- `hi`: Amount of time spent servicing hardware interrupts.
- `si`: Amount of time spent servicing software interrupts.
- `st`: Amount of time lost due to running virtual machines (“steal time”).

The column headings in the process list are as follows:

- `PID`: The process's ID, a unique positive integer that identifies a process.
- `USER`: This is the "effective" username (which maps to a user ID) of the user who started the process. Linux assigns a real user ID and an effective user ID to processes; the latter allows a process to act on behalf of another user.
- `PR and NI`: `NI` field shows the "nice" value of a process. The `PR` field shows scheduling priority of the process from the perspective of the kernel. The nice value affects the priority of a process.
- `VIRT, RES, SHR and %MEM`: These 3 fields are related with memory consumption of the processes. `VIRT` is the total amount of memory consumed by a process. This includes the program's code, the data stored by the process in memory, as well as any regions of memory that have been swapped to the disk. `RES` is the memory consumed by the process in RAM, and `%MEM` expresses this value as a percentage of the total RAM available. Finally, `SHR` is the amount of memory shared with other processes.
- `S`: A process maybe in various states. This field shows the process state in the single-letter form.
- `TIME+`: The total CPU time used by the process since it started, precise to the hundredths of a second.
- `COMMAND`: Show the name of the processes.

The status of the process can be one of the following:

- `D`: Uninteruptible sleep
- `R`: Running
- `T`: Traced(stopped)
- `Z`: Zombie

Press Q to exit top

**2. Scrolling the Display**

We can press the Up or Down Arrows, Home, End, and Page Up or Page Down keys to move up and move down and access the processes.

Press the Left or Right Arrow to move the process list sideways. This is useful to see any columns that don't fit within the confines of the terminal window.


**3. Changing the Numeric Units**

To change the display units to sensible values. Press capital E to cycle through the units used to display memory values in these options: kibibytes, mebibytes, tebibytes, pebibytes, and exbibytes. The unit in use is the first item on lines four and five.

Press lowercase "e" to do the same thing for the values in the process list: kibibytes, mebibytes, gibibytes, tebibytes, and pebibytes.

We pressed E to set the dashboard memory units to gibibytes and “e” to set the process list memory units to mebibytes.

![image](https://user-images.githubusercontent.com/55236614/112093046-5d9b1a80-8bcb-11eb-8552-3d0ff1c3b89b.png)


**4. Changing the Summary Contents**

We can change the display settings for the lines in the dashboard or remove them completely.

Press l to toggle the load summary line (the first line) on or off. We removed the load summary line in the image below.

![image](https://user-images.githubusercontent.com/55236614/112093117-70adea80-8bcb-11eb-8f4d-b9f7f23f1e34.png)


If we have a multi-core CPU, press 1 to change the display and individual statistics for each CPU.

![image](https://user-images.githubusercontent.com/55236614/112093135-7c011600-8bcb-11eb-8efd-04ed7ffbba0c.png)


We can press "t" again to change the graph display to solid block characters or simple ASCII, or press "t" once more to remove the CPU display and task summary line completely.

Press "m" to cycle the memory and swap memory lines through different display options 

![image](https://user-images.githubusercontent.com/55236614/112092926-23ca1400-8bcb-11eb-89f4-cc805c80e2d8.png)

	
**5. Color and Highlighting**

Press "z" to add color to the display.

![image](https://user-images.githubusercontent.com/55236614/112093272-b4085900-8bcb-11eb-8452-7b51a8a3e854.png)

Press "y" to highlight running tasks in the process list. Pressing "x" highlights the column used to sort the process list. we can toggle between bold and reversed text highlighting by pressing "b".

![image](https://user-images.githubusercontent.com/55236614/112093429-f3cf4080-8bcb-11eb-8b61-c255ad485a31.png)

	
**6. Sorting by Columns**

By default, the process list is sorted by the %CPU column. You can change the sort column by pressing the following:

- P: The %CPU column.
- M: The %MEM column.
- N: The PID column.
- T: The TIME+ column.

The image below show the process list which is sorted by the %MEM column.

![image](https://user-images.githubusercontent.com/55236614/112093941-e2d2ff00-8bcc-11eb-908d-6a7eb422c5df.png)


**7. See the Full Command Line**

Pressing "c" toggles the COMMAND column between displaying the process name and the full command line.

![image](https://user-images.githubusercontent.com/55236614/112094087-33e2f300-8bcd-11eb-801b-7c2a66385530.png)

To see a "tree" of processes that were launched or spawned by other processes, press V

**8. See Processes for a Single User**

Press "u" to see the processes for a single user. We will be prompted for the name or UID

![image](https://user-images.githubusercontent.com/55236614/112099896-de134880-8bd6-11eb-84f6-3f45d18f74f9.png)

We type the name and hit "Enter" to see the results.

**9. Only see Active Tasks**

Press "i" to see only active tasks

![image](https://user-images.githubusercontent.com/55236614/112100038-13b83180-8bd7-11eb-8c74-a757965388ce.png)

Task that have not consumed any CPU since the last update will not be shown.

**10. Set How Many Processes to Display**

Press "n" to limit the display to a certain number of lines. As you can see I typed "10" and hit "Enter". The results will be shown

![image](https://user-images.githubusercontent.com/55236614/112100257-6b569d00-8bd7-11eb-9fb2-345db642c862.png)


**11. Renice the Process**

We can press "r" to change the nice value (priority) for a process. We will be prompted for the process ID. Press enter to use the process ID of the task at the top of the process list. I typed 2020 as below

![image](https://user-images.githubusercontent.com/55236614/112100544-def8aa00-8bd7-11eb-8cae-6296228f0c92.png)

Next, we type the new nice value to apply to the prcess. I typed 20, and then press Enter

![image](https://user-images.githubusercontent.com/55236614/112100630-fc2d7880-8bd7-11eb-9889-d5abacb363fe.png)

The new nice value is applied to the process immediately

**12. Kill a Process**

Press "l" to kill a process. We will be prompted for the process ID we want to kill.

![image](https://user-images.githubusercontent.com/55236614/112101025-97265280-8bd8-11eb-9707-4df1778083bc.png)

I typed "7386" to kill the process which has COMMAND "Telegram" and press "Enter". Then we will be offered the chance to type the signal we want to send. If we just hit "Enter", top sends the SIGTERM (kill) signal

![image](https://user-images.githubusercontent.com/55236614/112101240-ea000a00-8bd8-11eb-81f4-e7d6492e5a83.png)

The chosen process was removed immediately.

**13. Customizing the Display**

We can also customize the colors and columns that are displayed. We’re going to change the color used for prompts, the default for which is red.

Press "Shift + Z" to go to the color settings page. To indicate which display element we want to change, press one of the following, which are case sensitive:

- `S`: Summary Data area.
- `M`: Messages and prompts.
- `H`: Column headings
- `T`: Task information in the process list

![image](https://user-images.githubusercontent.com/55236614/112102011-30a23400-8bda-11eb-8407-4624f198be3b.png)

We can also change the columns displayed in the Fields Management screen. Press F to enter the Fields Management screen.

![image](https://user-images.githubusercontent.com/55236614/112103079-d2765080-8bdb-11eb-8b0c-d85dffbc4b6e.png)

While the highlight is on the UID column, we press “s” to sort the process list on the UID column.

Press "s" and "Enter" to save the selection. Then proess "q" to leave the Fields Management screen.

![image](https://user-images.githubusercontent.com/55236614/112103257-12d5ce80-8bdc-11eb-99ee-0c50bd711d82.png)

**14. Alternative Display Mode**

This works best in full-screen mode. Press A to display four areas in the process list, and then press "a" to move from area to area.

![image](https://user-images.githubusercontent.com/55236614/112103545-7eb83700-8bdc-11eb-901c-6a8bdf6acc6c.png)

Each area has a different collection of columns, but each is also customizable through the Fields Management screen. This gives you scope to have a full-screen, customized display showing different information in each area, and the ability to sort each area by a different column.

**15. Other keystrokes**

The following are some other keys we might find useful in top:

- `W`: Save you settings and customizations so they will still be in effect when we next start top.
- `d`: Set a new display refresh rate.
- `Space`: Force top to refresh its display right now.

Reference: https://www.tecmint.com/12-top-command-examples-in-linux/

### `vmstat`

> Linux provides a way for you to monitor all of this activity in the shape of the vmstat command, which reports on virtual memory statistics.


**1. The vmstat Command**

Just type `vmstat` and press "Enter", it will show us a set of values. Theses values are the averages for each of the statistics since our computer was last rebooted. 

![image](https://user-images.githubusercontent.com/55236614/112105840-76153000-8bdf-11eb-9da7-6057c1e56692.png)

Here is a list of the data items in each comlumn:

`Proc`

- `r`: The number of runnable processes. These are processes that have been launched and are either running or are waiting for their next time-sliced burst of CPU cycles.
- `b`: The number of processes in uninterruptible sleep. The process isn’t sleeping, it is performing a blocking system call, and it cannot be interrupted until it has completed its current action. Typically the process is a device driver waiting for some resource to come free. Any queued interrupts for that process are handled when the process resumes its usual activity.

`Memory`

- `swpd`: the amount of virtual memory used. In other words, how much memory has been swapped out.,
- `free`: the amount of idle (currently unused) memory.
- `buff`: the amount of memory used as buffers.
- `cache`: the amount of memory used as cache.

`Swap`

- `si`: Amount of virtual memory swapped in from swap space.
- `so`: Amount of virtual memory swapped out to swap space.

`IO`

- `bi`: Blocks received from a block device. The number of data blocks used to swap virtual memory back into RAM.
- `bo`: Blocks sent to a block device. The number of data blocks used to swap virtual memory out of RAM and into swap space.

`System`

- `in`: The number of interrupts per second, including the clock.
- `cs`: The number of context switches per second. A context switch is when the kernel swaps from system mode processing into user mode processing.

`CPU`

These values are all percentages of the total CPU time.

- `us`: Time spent running non-kernel code. That is, how much time is spent in user time processing and in nice time processing.
- `sy`: Time spent running kernel code.
- `id`: Time spent idle.
- `wa`: Time spent waiting for input or output.
- `st`: Time stolen from a virtual machine. This is the time a virtual machine has to wait for the hypervisor to finish servicing other virtual machines before it can come back and attend to this virtual machine.


**2. Using a Time Interval**

We can have `vmstat` provide regular updates to these figures by using a delay value in seconds. 

![image](https://user-images.githubusercontent.com/55236614/112107516-9f36c000-8be1-11eb-8afc-3a0bfe84ad98.png)

Every five seconds as above figure, `vmstat` will add another line of data to the table. We will need to press Ctrl+C to stop this.
	

**3. Using a Count Value**

To have `vmstat` provide an update every three seconds-but only for four updates-use the following command:

![image](https://user-images.githubusercontent.com/55236614/112107423-8201f180-8be1-11eb-80b7-516d3387b31a.png)

After four updates `vmstat` stops of its own accord.

**4. Changing the Units**

We can choose to have the memory and swap statistics displayed in kilobytes or megabytes using the -S (unit-character) option. This must be followed by one of k, K, m, M. These represent:

- k: 1000 bytes.
- K: 1024 bytes.
- m: 1000000 bytes.
- M: 1048576 bytes.

To have the statistics updated every 5 seconds with the memory and swap statistics displayed in megabytes, use the following command:

![image](https://user-images.githubusercontent.com/55236614/112107915-26843380-8be2-11eb-833c-82a134bbce88.png)


**5. Active and Inactive Memory**

If you use the -a (active) option the buff and cache memory columns are replaced by the “inact” and “active” columns. As they would suggest, these show the amount of inactive and active memory.

To see these two columns instead of the buff and cache columns, include the -a option, as shown:

![image](https://user-images.githubusercontent.com/55236614/112108204-75ca6400-8be2-11eb-9909-2c80c10d4b5b.png)

The inact and active columns are affected by the -S (unit-character) option.
	
**6. Forks**

The -f switch displays the number of forks that have happened since the computer was booted up.

This shows the number of tasks that have been launched (and, for the bulk of them, closed again) since the system was booted. Every process launched from the command line would increase this figure. Each time a task or process spawns or clones a new task, this figure will increase.

![image](https://user-images.githubusercontent.com/55236614/112108432-c772ee80-8be2-11eb-95ce-1975a22312c7.png)

**7. Displaying Slabinfo**

The kernel has its own memory management to worry about as well as the memory management for the operating system and all of the applications.

As you might imagine the kernel is allocating and deallocating memory over and over for the many different types of data object that it must handle. To make this as efficient as possible, it uses a system called slabs. This is a form of caching.

Memory allocated, used, and no longer required for a specific type of kernel data object can be re-used for another data object of the same type without the memory being deallocated and reallocated. Think of slabs as pre-allocated, made to measure, segments of RAM for the kernel’s own needs.

To see the statistics for the slabs, use the -m (slabs) option. You will need to use sudo, and you will be prompted for your password. As the output can be quite lengthy, we are piping it through less.

![image](https://user-images.githubusercontent.com/55236614/112108559-f12c1580-8be2-11eb-803b-80ad96f2cbed.png)

The output has five columns. These are:

- Cache: Name of the cache
- num: The number of currently active objects in this cache.
- total: The total number of available objects in this cache.
- size: The size of each object in the cache.
- pages: The total number of memory pages that have (at least) one object currently associated with this cache.

Press q to 

**8. Displaying Event Counters and Memory Statistics**

To display a page of event counters and memory statistics, use the -s (stats) option. 

![image](https://user-images.githubusercontent.com/55236614/112108823-508a2580-8be3-11eb-93d4-4d793fdc9495.png)

**9. Displaying Disk Statistics**

We can obtain a similar listing of disk statistics using the -d(disk) option.

For each disk, 3 columns are displayed, these are Reads, Writes, and IO.

![image](https://user-images.githubusercontent.com/55236614/112108958-80d1c400-8be3-11eb-9e42-dd95e9f34103.png)

This is what the columns mean:

`Reads`

- `total`: The total count of disk reads.
- `merged`: The total count of grouped reads.
- `sectors`: The total count of sectors that have been read in.
- `ms`: Total count of time in milliseconds that were used reading data from the disk.

`writes`

- `total`: The total count of disk writes.
- `merged`: The total count of grouped writes.
- `sectors`: The total count of sectors written to.
- `ms` = Total count of time in milliseconds that were used writing data to the disk.

`IO`

- `cur`: Number of current disk reads or writes.
- `sec`: Time spent in seconds for any in-progress reads or writes.

**10. Displaying Summary Disk Statistics**

To see a quick display of summary statistics for our disk activity, use the -D(disk-sum) option.

The number of disks might look abnormally high. With Ubuntu (currently OS), each time you install an application from a Snap, a squashfs pseudo-filesystem is created which is attached to a /dev/loop device

Annoyingly these device entries are counted as hard drive devices by many of the Linux commands and utilities.

![image](https://user-images.githubusercontent.com/55236614/112109416-21c07f00-8be4-11eb-97f4-739dac583987.png)

**11. Displaying Partition Statistics**

To see statistics related to a specific partition, use the -p (partition) option and provide the partition identifier as a command line parameter.

Here we are going to look at the partition sda1. The digit one indicates this is the first partition on device sda, which is the main hard drive for this computer.

The information returned shows the total count of disk reads and disk writes to and from that partition, and the number of sectors included in disk read and disk write actions.

![image](https://user-images.githubusercontent.com/55236614/112109694-76fc9080-8be4-11eb-8554-edf713693b2f.png)

Reference: https://www.howtogeek.com/424334/how-to-use-the-vmstat-command-on-linux/

### `lsof`

> Linux/Unix consider everything as file and maintains folder. So **“Files or a File ”** is very important in Linux/Unix. While working in Linux/Unix system there might be several file and folder which are being used, some of them would be visible and some not.
lsof command stands for **List Of Open File**. This command provides a list of files that are opened. Basically, it gives the information to find out the files which are opened by which process. With one go it lists out all open files in output console. It cannot only list common regular files but it can list a directory, a block special file, a shared library, a character special file, a regular pipe, a named pipe, an internet socket, a UNIX domain socket, and many others. it can be combined with grep command can be used to do advanced searching and listing.

**1. List all open files**

`lsof`

![image](https://user-images.githubusercontent.com/55236614/112274083-a8db2900-8cb0-11eb-83ee-5a6e14a887d2.png)

Here, you observe there are details of files which are opened. Process Id, the user associated with the process, FD(file descriptor), size of the file all together gives detailed information about the file opened by the command, process ID, user, its size etc.

- `FD` represents as File descripter.
- `cwd` : Current working directory.
- `txt` : Text file.
- `mem` : Memory file.
- `mmap` : Memory mapped device.

**2. List all files opened by a user**

There are several users of a system and each user have different requirements and accordingly they use files and devices. To find a list of files that are opened by a specific user this command is useful.

`lsof -u buichidung`

![image](https://user-images.githubusercontent.com/55236614/112276289-26079d80-8cb3-11eb-95bc-99bb1fd5790c.png)

In the figure given above with the command lsof -u ubuntu lists out all the files opened by ubuntu user. Along with that we can see the type of file here and they are:

- `DIR`: Directory
- `REG`: Regular file
- `CHR`: Character special file

**3. List all files which are opened by everyone except a specific user**

`lsof -u ^root`

![image](https://user-images.githubusercontent.com/55236614/112276635-8bf42500-8cb3-11eb-8df5-7b7dea1524ad.png)

In the given figure we can observe there are no files that are opened by the root user.

**4. List all open files by a particular Process**

`lsof -c mysql`

![image](https://user-images.githubusercontent.com/55236614/112276981-dc6b8280-8cb3-11eb-8ab1-7e0bdfccfea6.png)

**5. List all open files that are opened by a particular process**

`lsof -p ID`

![image](https://user-images.githubusercontent.com/55236614/112277443-54d24380-8cb4-11eb-8028-3be47194546d.png)

Each file is associated with some process ID. There can be many files that are opened by a particular process.

**6. Files opened by all other PID**

As the above-given figure command lists out the files opened by a particular process ID.

`lsof -p ^ID`

**7. List parent process IDs**

There is a large number of process running in a system and they have files opened for its usage. There may be many child processes of a process and this process can also be termed as the parent process. To find out the list of files opened by parent process Id lsof command is used with the option -R.

`lsof -R`

![image](https://user-images.githubusercontent.com/55236614/112279493-906e0d00-8cb6-11eb-9958-f476aca83145.png)

**8. Files opened by a directory**

 It lists out the files which are opened by a particular directory. There are files as well as the directory in a system. So there can be several files opened by a directory as well as the regular file.
 
 ![image](https://user-images.githubusercontent.com/55236614/112279697-c7dcb980-8cb6-11eb-9953-3d6239845ff3.png)

**9. Files opened by network connections**

Our Pc/system can be connected through various networks which helps in a variety of purpose. As we know that in Linux everything is a file, so we can even check the files that are opened by some network connections in the system.

`lsof -i`

![image](https://user-images.githubusercontent.com/55236614/112279819-e8a50f00-8cb6-11eb-9891-b89dcda498c9.png)

Here in the figure, we can see the files opened by the TCP network. In the same way, we can check for UDP etc.

Reference: https://www.geeksforgeeks.org/lsof-command-in-linux-with-examples/


### `tcpdump`

> `tcpdump`: a packet sniffing and packet analyzing tool for a System Administrator to troubleshoot connectivity issues in Linux. It is used to capture filter, and analyze network traffic such as TCP/IP packets going through our system. It is many times used as a security tool as well as. It saves the captured in a pcap life, these pcap files can then be opened through Wireshark or through the command tool itself.

#### **Installing tcpdump tool in Linux**

**For RedHat based linux OS**

`yum install tcpdump`

**For Ubuntu/Debian OS**

`apt install tcpdump`

**1. Capture the packets of current network interface**
	
`sudo tcpdump`

![image](https://user-images.githubusercontent.com/55236614/112112881-ac0ae200-8be8-11eb-9fdb-d53498e345ad.png)

This will capture the packets from the current interface of the network through which the system is connected to the internet.
	
**2. Capture packets from a specific network interface**

`sudo tcpdump -i vmnet1`

![image](https://user-images.githubusercontent.com/55236614/112116321-c050de00-8bec-11eb-803a-db3563346827.png)

The command will now capture the packets from vmnet1 network interface.

**3. Capture specific number of packets**
	
`sudo tcpdump -c 5 -i vmnet1`

![image](https://user-images.githubusercontent.com/55236614/112117907-6ea95300-8bee-11eb-8cae-e39a0aa962ca.png)

This command will capture only 5 packets from the vmnet1 interface.

**4. Print captured packages in ASCII format**

`sudo tcpdump -A -i vmnet1`

![image](https://user-images.githubusercontent.com/55236614/112118072-98627a00-8bee-11eb-8041-a71cc521ac60.png)

This command will now print the captured packets from vmnet1  to ASCII value.

**5. Display all available interfaces**

`sudo tcpdump -D`

![image](https://user-images.githubusercontent.com/55236614/112118217-b7610c00-8bee-11eb-9259-674a42bd4b8c.png)

This command will display all the interfaces that are available in the system.

**6. Display packets in HEX and ASCII values**

`sudo tcpdump -XX -i enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112118461-fbeca780-8bee-11eb-89ac-b9f918f03be9.png)

This command will now print the packages captured from the enp1s0 interface in the HEX and ASCII values.

**7. Save captured packets into a file**

`sudo tcpdump -w captured_packets.pcap -i enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112119359-e1ff9480-8bef-11eb-800d-394a342722c5.png)

This command will now output all the captures packets in a file named as captured_packets.pcap

**8. Read captured packets from a file**

`sudo tcpdump -r captured_packets.pcap`

![image](https://user-images.githubusercontent.com/55236614/112122690-1e80bf80-8bf3-11eb-92af-f4ce17e1e0a5.png)

This command will now read the captured packets from the captured_packets.pcap file

**9. Capture packets with ip address**

`sudo tcpdump -n -i enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112123362-c6968880-8bf3-11eb-9aef-223b215dd8d6.png)

This command will now capture the packets with IP addresses.

**10. Capture only TCP packets**

`sudo tcpdump -i enp1s0 tcp`

![image](https://user-images.githubusercontent.com/55236614/112124086-8e437a00-8bf4-11eb-94ff-4910317383e6.png)	

This command will now capture only TCP packets from enp1s0

Reference: https://www.tecmint.com/12-tcpdump-commands-a-network-sniffer-tool/

### `netstat`

> Netstat command displays various network related information such as network connections, routing tables, interface statistics, masquerade connections, multicast memberships etc.,

#### Examples:

**1. Show both listening and non-listening sockets**

`netstat -a` or `netstat -all`

![image](https://user-images.githubusercontent.com/55236614/112127265-d617d080-8bf7-11eb-999c-7c73df296567.png)

![image](https://user-images.githubusercontent.com/55236614/112127540-22631080-8bf8-11eb-80ee-4abf30a233a5.png)

**2. List all tcp ports**

`netstat -at`

![image](https://user-images.githubusercontent.com/55236614/112127709-4c1c3780-8bf8-11eb-90f0-3b63708abfe5.png)
	
**3. List all udp ports**

`netstat -au`

![image](https://user-images.githubusercontent.com/55236614/112127837-6b1ac980-8bf8-11eb-84ba-a0713a0a5697.png)
	
**4. List only listening ports**

`netstat -l`

![image](https://user-images.githubusercontent.com/55236614/112128053-9f8e8580-8bf8-11eb-8b47-c94f2a16acf7.png)

**5. List only listening TCP ports**
	
`netstat -lt`

![image](https://user-images.githubusercontent.com/55236614/112128200-c482f880-8bf8-11eb-8556-f04292180eda.png)

**6. List only listening UDP ports**

`netstat -lu`

![image](https://user-images.githubusercontent.com/55236614/112128289-dcf31300-8bf8-11eb-9c51-1d5a736bca68.png)

**7. List only the listening UNIX ports**

`netstat -lx`

![image](https://user-images.githubusercontent.com/55236614/112128378-f4320080-8bf8-11eb-9fd1-cb78765dde4d.png)

**8. List the statistics for all ports**

`netstat -s`

![image](https://user-images.githubusercontent.com/55236614/112128479-0dd34800-8bf9-11eb-9282-1c411e5fb1cc.png)

**9. List the statistics for TCP (or) UDP ports**

`netstat -st` - (TCP)

![image](https://user-images.githubusercontent.com/55236614/112128631-32c7bb00-8bf9-11eb-84cc-0c6970e09ab1.png)

`netstat -su` - (UDP)

![image](https://user-images.githubusercontent.com/55236614/112128760-512db680-8bf9-11eb-9e28-0e8d7cc612a0.png)
	
**10. Display PID and program names in the output**

`netstat -pt`

![image](https://user-images.githubusercontent.com/55236614/112128877-6e628500-8bf9-11eb-9e1b-6a2aa9c5a142.png)
	
**11. Print the netstat information continuously**

`netstat -c`

![image](https://user-images.githubusercontent.com/55236614/112129188-bc778880-8bf9-11eb-8635-1657a2285724.png)

**12. The non-supportive address families in the system**

`netstat --verbose`

![image](https://user-images.githubusercontent.com/55236614/112129417-f9dc1600-8bf9-11eb-96ad-eccaa7a76a4b.png)

At the end

![image](https://user-images.githubusercontent.com/55236614/112129476-09f3f580-8bfa-11eb-99e4-cac57a260067.png)
	
**13. The kernel routing information**

`netstat -r`

![image](https://user-images.githubusercontent.com/55236614/112129559-1c6e2f00-8bfa-11eb-8fdf-bd42817f6548.png)

**14. The port on which program is running**

`netstat -ap | grep ssh`

![image](https://user-images.githubusercontent.com/55236614/112129674-3b6cc100-8bfa-11eb-8080-156262b6b9df.png)

**15. Which process is using a particular port**

`netstat -an | grep ':80'`

![image](https://user-images.githubusercontent.com/55236614/112129781-5808f900-8bfa-11eb-9b6d-b81a9a5a709b.png)

**16. List of network interfaces**

`netstat -i`

![image](https://user-images.githubusercontent.com/55236614/112129843-69520580-8bfa-11eb-834b-3d561c81bf78.png)

Display extended information on the interfaces (similar to ifconfig) using `netstat -ie`:

![image](https://user-images.githubusercontent.com/55236614/112130114-b209be80-8bfa-11eb-9208-704d2d83f5e8.png)
	

References: 

- https://www.geeksforgeeks.org/netstat-command-linux/
	
	
### `htop`

> `htop` is a command line utility that allows the users to interactively monitor the system's vital resources or server's processes in realtime. `htop` is a newer version compared to `top` command, it offers many improvements over `top` command. `htop` support mouse operations, uses color in its output and gives visual indications about processor, memory and swap usage. It prints full command lines for processes and allows one to scroll both vertically and horizontally for processes and command lines respectively.

Installing on Ubuntu

`sudo apt-get install htop`

Installing on CentOS

`sudo yum install htop`

![image](https://user-images.githubusercontent.com/55236614/112244024-2d15b800-8c81-11eb-885e-f2f5f9f9162a.png)

**1. Options**

- `-d –delay` : Used to show the delay between updates, in tenths of seconds.
- `-C –no-color –no-colour` : Start htop in monochrome mode.
- `-h –help` : Used to display the help message and exit.
- `-u –user=USERNAME` : Used to show only the processes of a given user.
- `-p –pid=PID, PID…` : Used to show only the given PIDs.
- `-s –sort-key COLUMN` : Sort by this column (use –sort-key help for a column list).
- `-v –version` : Output version information and exit.

`htop -u buichidung`

![image](https://user-images.githubusercontent.com/55236614/112244258-972e5d00-8c81-11eb-8823-bec1df303345.png)

`htop -p 4625`

![image](https://user-images.githubusercontent.com/55236614/112244448-ef655f00-8c81-11eb-883e-ade681eed6bb.png)

**2. Interactive Commands**

![image](https://user-images.githubusercontent.com/55236614/112244512-06a44c80-8c82-11eb-9d4e-b41c73112638.png)

- `Arrows, Page Up, Page Down, Home, End`: Scroll the process list.
- `Space`: Tag or untag a process.
- `U` – Untag all processes (remove all tags added with the Space key).
- `s` – Trace process system calls.
- `F1` – Help
- `F2` – setup
- `F3` – search
- `F4` – filtering: type in part of a process command line and only processes whose names match will be shown.
- `F5` - Tree view
- `F6` – Sorting.
- `F7` – Increase the selected process’s priority. This can only be done by the superuser.
- `F8` – Decrease the selected process’s priority.
- `F9` – Kill process.
- `F10` – Quit.

Press `F5`:

![image](https://user-images.githubusercontent.com/55236614/112244821-63a00280-8c82-11eb-82eb-add83cbdd37e.png)

Press `F6` to sort bye `PID, USER, PRI, ...`:

![image](https://user-images.githubusercontent.com/55236614/112244890-80d4d100-8c82-11eb-86ff-cc1dfcf4d832.png)

Reference: https://www.geeksforgeeks.org/htop-command-in-linux-with-examples/
	
### `iotop`

> `iotop` or Input/Output top is a command in Linux which is used to display and monitor the disk IO usage details and even gets a table of existing IO utilization by the process. It is designed in python and needs kernel modules for its execution. It is used by system administrators to trace the specific process that may be causing a high disk I/O read/writes.
> 
> It requires a python interpreter for its execution. It produces output similar to that of top command. It generally requires root privileges for its execution.

Installating on CentOS/RHEL

`sudo yum install iotop`

Installating on Ubuntu

`sudo apt install iotop`

**1. To get the list of processes and their current disk IO usages**

`sudo iotop`

![image](https://user-images.githubusercontent.com/55236614/112254519-55a6ad80-8c93-11eb-957b-8fd1d41a8778.png)

This command will now display the list of processes and their current disk usage and will keep on updating the same.

**2. To show processes that are actually doing IO**

`sudo iotop -o`

![image](https://user-images.githubusercontent.com/55236614/112255370-a074f500-8c94-11eb-8bcb-0ad68473082f.png)

This will display all the processes which are currently and actually doing IO

**3. To get the version of the iotop**

`sudo iotop --version`

![image](https://user-images.githubusercontent.com/55236614/112255401-aec31100-8c94-11eb-9f3b-7eb5fedebce2.png)

This will display the currently installed version of iotop tool.

**4. To display help section**

`sudo iotop -h`

![image](https://user-images.githubusercontent.com/55236614/112255428-bda9c380-8c94-11eb-8218-379831086a47.png)

This command will display the help section of the iotop tool.

**5. To display output in non interactive mode**

`sudo iotop -b`

![image](https://user-images.githubusercontent.com/55236614/112255489-e16d0980-8c94-11eb-9fc5-3b6f19d513c1.png)

This will display the output in non-interactive and batch mode.

**6. To change the number of iterations or updations**

`sudo iotop -n 5`

![image](https://user-images.githubusercontent.com/55236614/112255550-006b9b80-8c95-11eb-97d8-703b716d69a2.png)

This command will not update the output 5 times in spite of the default time which is infinity.

**7. To display a specific process**

`sudo iotop -p 3093`

![image](https://user-images.githubusercontent.com/55236614/112255602-1da06a00-8c95-11eb-920c-f5a9d78d2b49.png)

This will display the IO usage of the process with the mentioned PID in spite of all the processes.

**8. To show accumulated output**

`sudo iotop -a`

![image](https://user-images.githubusercontent.com/55236614/112255622-285aff00-8c95-11eb-8b24-b4a4d98245f3.png)

This will not display the accumulated IO instead of bandwidth.

**9. To add a time stamp to each line**

`sudo iotop -t`

![image](https://user-images.githubusercontent.com/55236614/112255661-3a3ca200-8c95-11eb-96b2-702005b0175b.png)

This will add a time stamp to each line of the output.

**10. To suppress some lines of header**

`sudo iotop -q`

![image](https://user-images.githubusercontent.com/55236614/112255702-4de80880-8c95-11eb-8a7d-1d64dfcd1cf7.png)

This will now suppress some line of header in the output.

Reference: https://www.geeksforgeeks.org/iotop-command-in-linux-with-examples/#:~:text=iotop%20or%20Input%2FOutput%20top,kernel%20modules%20for%20its%20execution.

### `iostat`

> The `iostat` command in Linux is used for monitoring system input/output statistics for devices and partitions. It monitors system input/output by observing the time the devices are active in relation to their average transfer rates. The iostat produce reports may be used to change the system configuration to raised balance the input/output between the physical disks. iostat is being included in sysstat package. If you don’t have it, you need to install first.
 
Installing on Red Hat / CentOS / Fedora

`yum install sysstat`

Installing on Debian / Ubuntu / Linux Mint

`apt-get install sysstat`

**1. The iostat command in linux is used to get report and statistic**

`iostat`

![image](https://user-images.githubusercontent.com/55236614/112255984-dff01100-8c95-11eb-9a64-7bcf9f628380.png)

The **first section** contains CPU report:

- `%user` : It shows the percentage of CPU being utilization that while executing at the user level.
- `%nice` : It shows the percentage of CPU utilization that occurred while executing at the user level with a nice priority.
- `%system` : It shows the percentage of CPU utilization that occurred while executing at the system (kernel) level.
- `%iowait` : It shows the percentage of the time that the CPU or CPUs were idle during which the system had an outstanding disk I/O request.
- `%steal` : It shows the percentage of time being spent in involuntary wait by the virtual CPU or CPUs while the hypervisor was servicing by another virtual - processor.
- `%idle` : It shows the percentage of time that the CPU or CPUs were idle and the system did not have an outstanding disk I/O request.

The **second section** of the output contains device utilization report:

- `Device` : The device/partition name is listed in /dev directory.
- `tps` : The number of transfers per second that were issued to the device. Higher tps means the processor is busier.
- `Blk_read/s` : It shows the amount of data read from the device expressed in a number of blocks (kilobytes, megabytes) per second.
- `Blk_wrtn/s` : The amount of data written to the device expressed in a number of blocks (kilobytes, megabytes) per second.
- `Blk_read` : It shows the total number of blocks read.
- `Blk_wrtn` : It shows the total number of blocks written.

`iostat` uses **files** mentioned below to create the reports:

- `/proc/stat` Contains the system statistics.
- `/proc/uptime` Contains the system uptime.
- `/proc/diskstats` Contains the disks statistics.
- `/sys` Contains the statistics for block devices.
- `/proc/self/mountstats` Contains the statistics for network filesystems.
- `/dev/disk` Contains the persistent device names.

**2. iostat -x Command**

This command shows more details statistics information. `iostat` command gives I/O devices report utilization as a result. So it's possible to extend the statistic result for a diagnose in depth with the -x option.

`iostat -x`

![image](https://user-images.githubusercontent.com/55236614/112263706-5266ee00-8ca2-11eb-96dd-182cde9ada09.png)

- `avg-cpu`: It tells us the cpu block information.
- `Devide`: It tells us the device block information. To be sure that you are on the good device.
- `%util`: It tells us that how much time did the storage device have outstanding work (was busy).
- `svctm`: It indicates how fast does your I/O subsystem respond requests overall when busy. Actually, the less you load your system, higher svctm is.
- `await`: It indicates how fast do requests go through. It is just an average.
- `avgqu-sz`: It tells us how many requests are there in a request queue. Low = either your system is not going to be loaded, or has serialized I/O and cannot utilize underlying storage properly. High = your software stack is scalable enough to be loaded properly underlying I/O.
- `avgrq-sz`: It’s just an average request size which can indicate what kind of workload happens.
- `wsec/s` & `rsec/s`: It tells us the sectors read and written per second. Divide by 2048, and you’ll get the megabytes per second.
- `r/s` & `w/s`: It tells us read and write requests per second. These numbers are the ones that are the I/O capacity figures, though of course, depending on how much pressure underlying I/O subsystem gets (queue size!), they can vary.
- `rrqm/s & wrqm/s`: It tells us how many requests were merged by block layer.

**3. iostat -c Command**

This command show only the CPU statistic. It is possible to show the statistic information and report of our cpu with -c option.

`iostat -c`

![image](https://user-images.githubusercontent.com/55236614/112263753-66aaeb00-8ca2-11eb-9892-34693cbf6526.png)

**4. iostat -d Command**

This command displays only the device report. It is possible to only show the status of the device utilization with the help of -d option. It will be going to list information for each connected device.

`iostat -d`

![image](https://user-images.githubusercontent.com/55236614/112263933-a376e200-8ca2-11eb-8408-8d1fd4a3c65d.png)

**.5 iostat -xd Command**

This command shows us the extended I/O statistic for device only. We can display extended the statistic on one side and from the other side we can display the extended statistics. It means that we can display the extended statistics only for devices with the help of -xd option.

`iostat -xd`

![image](https://user-images.githubusercontent.com/55236614/112264095-f51f6c80-8ca2-11eb-992e-81cbba52f977.png)

**6. iostat -k Command**

This command captures the statistics in kilobytes or megabytes. By default, iostat measure the I/O system with the bytes unit. To make it easier to read, we can convert the iostat to show us reports in kilobytes or megabytes unit.

![image](https://user-images.githubusercontent.com/55236614/112264253-2dbf4600-8ca3-11eb-9946-50424fdeb7e2.png)

**7. iostat -m Command**

Used to create a report with megabytes unit

![image](https://user-images.githubusercontent.com/55236614/112264514-960e2780-8ca3-11eb-8c9b-29108d7a63f8.png)

**8. iostat -k 2 3 Command**

This command displays CPU and device statistics with delay. Same with the `vmstat`, as a statistic tool is the best way to use it with delay parameter. With the delay, we can see that what is the trend. 

![image](https://user-images.githubusercontent.com/55236614/112264838-09b03480-8ca4-11eb-9058-1a123a102337.png)
![image](https://user-images.githubusercontent.com/55236614/112264857-12086f80-8ca4-11eb-8f09-a948e251347c.png)
![image](https://user-images.githubusercontent.com/55236614/112264875-192f7d80-8ca4-11eb-96b8-5c7855e31893.png)

**9. iostat -c 1 2 Command**

To show CPU only report with 1 seconds interval and 2 times reports

![image](https://user-images.githubusercontent.com/55236614/112264967-43813b00-8ca4-11eb-8253-2c2e352237a7.png)

**10. iostat -d sda 2 2 Command**

Show sda device only report with 2 seconds interval and 2 times report.

![image](https://user-images.githubusercontent.com/55236614/112265142-922ed500-8ca4-11eb-8041-d70c63799be6.png)

**11. iostat -j ID loop18 sda -x -m 2 2 Command**

Display persistent device name statistics. It is possible to print the report by device name. It will be going to print the persistent device name with -j parameter and adding ID keyword.

![image](https://user-images.githubusercontent.com/55236614/112265485-2600a100-8ca5-11eb-84ac-44b7c5c1c4de.png)
![image](https://user-images.githubusercontent.com/55236614/112265508-2e58dc00-8ca5-11eb-8466-12aa794432f1.png)

**12. iostat -p Command**

This command display statistics for block devices. With the help of this command, it is possible to directly show information for each block device automatically. No need to indicate each of the device names. It will display statistics for block devices and all their partitions that are being used by the system. If a device name is entered on the command line, then the statistics for it and all of its partitions are displayed. Just use this -p option.

![image](https://user-images.githubusercontent.com/55236614/112265565-44669c80-8ca5-11eb-8f78-ceb517bd58a4.png)

**13. iostat -N command**

Display lvm2 statistic information. It is possible to view the LVN statistic with -N option. This command displays the registered device mapper names for any of the device mapper devices.

![image](https://user-images.githubusercontent.com/55236614/112268076-feabd300-8ca8-11eb-9ff2-033247d882d1.png)

Reference: https://www.geeksforgeeks.org/iostat-command-in-linux-with-examples/

### `iptraf-ng`

> **IPTraf** is an ncurses-based **IP LAN** monitoring tool (text-based) wherein we can monitor various connections like **TCP, UDP, ICMP, non-IP** counts and also **Ethernet** load information etc.

Installating on CentOS/RHEL

`sudo yum install iptraf`

Installating on Ubuntu

`sudo apt install iptraf`

![image](https://user-images.githubusercontent.com/55236614/112269937-a2967e00-8cab-11eb-9d22-4c4b6549788d.png)

**1. IP traffic monitor**

![image](https://user-images.githubusercontent.com/55236614/112270042-be9a1f80-8cab-11eb-9eeb-c74164ce2eda.png)

**2. General interface statistics**

![image](https://user-images.githubusercontent.com/55236614/112270163-df627500-8cab-11eb-9aba-ef6c28c68750.png)

**3. Detailed interface statistics**

![image](https://user-images.githubusercontent.com/55236614/112270282-09b43280-8cac-11eb-9255-a65092903fba.png)

**4. Statistical breakdowns**

![image](https://user-images.githubusercontent.com/55236614/112270382-2c464b80-8cac-11eb-854a-afb71bce6cfb.png)

**5. LAN station monitor**

![image](https://user-images.githubusercontent.com/55236614/112270530-5e57ad80-8cac-11eb-9ea3-62e0b38958c7.png)

**6. Configure**

![image](https://user-images.githubusercontent.com/55236614/112270569-6ca5c980-8cac-11eb-90df-6355b9104bd5.png)

**7. IPTraf Options**

`sudo iptraf-ng -i enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112270754-aaa2ed80-8cac-11eb-8d4b-9c1319be741c.png)

Using `iptraf-ng -i` will immediately start the IP traffic monitor on a particular interface. For example, the following command will start the IP traffic on interface enp1s0. This is the primary interface card that attached to your system. Else you can aloso monitor all your network interface traffic using argument as `iptraf-ng -i all`

`sudo iptraf-ng -s enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112270867-cc9c7000-8cac-11eb-8312-042206c93ae9.png)

And we cal also monitor **TCP/UDP** traffic on a specific interface as above

Reference: https://www.tecmint.com/real-time-interactive-ip-lan-monitoring-with-iptraf-tool/

### `iftop`

> **iftop** is a network analyzing tool used by system administrators to view the bandwidth related stats. It shows a quick overview of the networking activities on an interface. It stands from Interface TOP and the top is derived from op command in Linux. It even acts as a diagnostics to diagnose which program is causing the problem to the network.

Installing on RedHat based Linux

`yum install epel-release`

`yum install iftop`

Installing on Debian / Ubuntu

`sudo apt-get install iftop`

**1. Display basic bandwidth uage of the default interface**

`sudo iftop`

![image](https://user-images.githubusercontent.com/55236614/112272056-45e89280-8cae-11eb-85dd-af7b82edccd5.png)


**2. Display bandwidth details of a specific router**

`sudo iftop -i enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112272220-7c261200-8cae-11eb-9a30-e99a3a6d4881.png)

**3. Stop hostname lookup**

`sudo iftop -n -i enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112272320-a1b31b80-8cae-11eb-80db-83161f1448ee.png)

**4. Stop the conversion of port number to services**

`sudo iftop -N -i enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112272448-c909e880-8cae-11eb-8edc-96dbd9c2804f.png)

**5. Stop the display bargraph**

`sudo iftop -b`

![image](https://user-images.githubusercontent.com/55236614/112272518-e343c680-8cae-11eb-9bc3-7620b6fbdfa6.png)

**6. Display the text interface without ncurses**

`sudo iftop -t`

![image](https://user-images.githubusercontent.com/55236614/112272613-05d5df80-8caf-11eb-8c5b-e9e48686aa0b.png)

**7. Sort output by source address**

`sudo iftop -o source`

![image](https://user-images.githubusercontent.com/55236614/112272720-2140ea80-8caf-11eb-8489-bbba5eafc0b2.png)


**8. Sort output by destinition address**

`sudo iftop -o destination`

![image](https://user-images.githubusercontent.com/55236614/112272820-3e75b900-8caf-11eb-8430-107597ba3382.png)

**9. Specify number of lines to be printed**

`sudo iftop -L 2 -i enp1s0`

![image](https://user-images.githubusercontent.com/55236614/112272969-6107d200-8caf-11eb-8605-5ebca2fc09dd.png)

Print 2 output lines of statistics or bandwidth usage and not more than 2 lines would be printed.

**10. Display help**

`sudo iftop -h`

![image](https://user-images.githubusercontent.com/55236614/112273134-96acbb00-8caf-11eb-8552-a1e242add91d.png)

Reference: https://www.geeksforgeeks.org/iftop-command-in-linux-with-examples/


***

## *Installation Packages Management*
<details>
<summary>Quản lý gói cài đặt</summary>
</details>

***

## *Log Server Management*
<details>
<summary>Quản lý log server</summary>
</details>

***

## *Console*
<details>
<summary>Trình soạn thảo console</summary>
</details>

***

## *Network and IPTables*
<details>
<summary>Quản lý Network và IPTables</summary>
</details>

***

## *Web Server Installation*
<details>
<summary>Cài đặt web server</summary>
</details>

***

## *Services and Network in Host*
<details>
<summary>Quản lý services/network tại máy chủ</summary>
</details>

***

## *Other skills*
<details>
<summary>Các kỹ năng khác</summary>
</details>

***

## *References*
<details>
<summary>References</summary>
</details>
