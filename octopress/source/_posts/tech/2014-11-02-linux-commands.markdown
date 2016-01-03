---
layout: post_tech
title: "Linux Commands"
date: 2014-11-02 14:10:18 +0800
comments: true
categories: [tech]
tags: [linux, shell]
---

## 1. Linux File System

```
/
/boot

/etc

/bin, /usr/bin
/sbin, /usr/sbin
/usr
/usr/local

/var
/lib

/home
/root

/tmp

/dev
/proc
/media,/mnt
```

## 2. Shell

### 2.1. Navigation

```bash
$ pwd                      # working directory

$ ls                       # list
$ ls -l
$ ls -la

$ cd                       
```

### 2.2. Manipulating files

```bash
$ cp
$ cp -i                          # interactive
$ cp -R                          # all

$ mv
$ mv -i

$ rm
$ rm -i
$ rm -r

$ mkdir
```

### 2.3. Working with commands

```bash
$ type                           # display info about the cmd 
$ type type
$ type ls
$ type cp

$ which                          # locate a cmd
$ which ls

$ help                           # display reference for shell builtin
$ help -m cd
$ mkdir --help

$ man                            # display an on-line cmd reference
$ man ls
```


### 2.4. I/O redirection

```bash
$ ls > list_file.txt                             # output with overwritten
$ ls >> list_file.txt                            # output with appending

$ sort < file_list.txt                           # input
$ sort < file_list.txt > sorted_file_list.txt    # input and then output


# pipeline
$ ls -l | less
$ ls -lt | head                                  # display the 10 newest files
$ du | sort -nr
$ find . -type f -print | wc -l

# filters
$ sort
$ uniq
$ grep
$ fmt
$ pr
$ head/tail
$ tr
$ sed
$ awk

$ cat poorly_formatted_report.txt | fmt | pr | lpr
$ cat unsorted_list_with_dupes.txt | sort | uniq | pr | lpr
$ tar tzvf name_of_file.tar.gz | less
```

### 2.5. Expansion

```bash
$ echo this is a test
$ echo *

$ ls
$ echo D*
$ echo *s
$ echo [[:upper:]]*
$ echo /usr/*/share

$ echo ~
$ echo ~foo
$ echo $((2 + 2))

$ echo Front-{A,B,C}-Back
$ echo Number_{1..5}
$ echo {Z..A}
$ echo a{A{1,2},B{3,4}}b
$ echo a{A{1,2},B{3,4}}b

$ echo $USER

$ echo $(ls)
$ ls -l $(which cp)
$ file $(ls /usr/bin/* | grep bin/zip)
```

### 2.6. Permissions

`-rwxrwxrwx`

- `-`: file type
- `rwx`: read, write, execute, first - file owner, second - group owner, third - other users

```
rwx rwx rwx 111 111 111 777  
rw- rw- rw- 110 110 110 666  
rwx --- --- 111 000 000 700  

rwxrwxrwx 777  
rwxr-xr-x 755  
rwx------ 700  
rw-rw-rw- 666  
rw-r--r-- 644  
rw------- 600  
```

commands

```bash
$ chmod
$ chown
$ chgrp
$ su
$ sudo 
```

### 2.7. Job control

```bash
$ ps                 # list the processes running on the system

$ kill               # send a signal to one or more processes (usually to "kill" a process)
$ kill -1/SIGHUP
$ kill -2/SIGINT
$ kill -15/SIGTERM
$ kill -9/SIGKILL

$ jobs               # an alternate way of listing your own processes
$ bg                 # put a process in the background
$ fg                 # put a process in the forground
```


## 2. Shell Scripts

```bash
#!/bin/bash
```

### 2.1. Variables

```bash
#!/bin/bash

# sysinfo_page - A script to produce an HTML file

title="System Information for $HOSTNAME"
RIGHT_NOW=$(date +"%x %r %Z")
TIME_STAMP="Updated on $RIGHT_NOW by $USER"

cat <<- _EOF_
    <html>
    <head>
        <title>
        $title
        </title>
    </head>

    <body>
    <h1>$title</h1>
    <p>$TIME_STAMP</p>
    </body>
    </html>
_EOF_      
```


### 2.2. shell functions

```bash
#!/bin/bash

# sysinfo_page - A script to produce an system information HTML file

##### Constants

TITLE="System Information for $HOSTNAME"
RIGHT_NOW=$(date +"%x %r %Z")
TIME_STAMP="Updated on $RIGHT_NOW by $USER"

##### Functions

system_info()
{
    # Temporary function stub
    echo "function system_info"
}


show_uptime()
{
    # Temporary function stub
    echo "function show_uptime"
}


drive_space()
{
    # Temporary function stub
    echo "function drive_space"
}


home_space()
{
    # Temporary function stub
    echo "function home_space"
}


##### Main

cat <<- _EOF_
  <html>
  <head>
      <title>$TITLE</title>
  </head>

  <body>
      <h1>$TITLE</h1>
      <p>$TIME_STAMP</p>
      $(system_info)
      $(show_uptime)
      $(drive_space)
      $(home_space)
  </body>
  </html>
_EOF_ 
```

### 2.3. flow control

#### 2.3.1. if 

```bash if
if [ -f .bash_profile ]; then
    echo "You have a .bash_profile. Things are fine."
else
    echo "Yikes! You have no .bash_profile!"
fi


# Alternate form

if [ -f .bash_profile ]
then
    echo "You have a .bash_profile. Things are fine."
else
    echo "Yikes! You have no .bash_profile!"
fi

# Another alternate form

if [ -f .bash_profile ]
then echo "You have a .bash_profile. Things are fine."
else echo "Yikes! You have no .bash_profile!"
fi
```

test

```bash
-d file         True if file is a directory
-e file         True if file exists
-f file         True if file exists and is a regular file
-L file         True if file is a symbolic link
-r file         True if file is a file readable by you
-w file         True if file is a file writable by you
-x file         True if file is a file executable by you
file1 -nt file2 True if file1 is newer than (according to modification time) file2
file1 -ot file2 True if file1 is older than file2
-z string       True if string is empty
-n string       True if string is not empty
str1 = str2     True if string1 equals string2
str1 != str2    True if string1 does not equal string2
```

exit

```
0       success
1       failure
```

#### 2.3.2. case

```bash
#!/bin/bash

echo -n "Type a digit or a letter > "
read character
case $character in
                                # Check for letters
    [[:lower:]] | [[:upper:]] ) echo "You typed the letter $character"
                                ;;

                                # Check for digits
    [0-9] )                     echo "You typed the digit $character"
                                ;;

                                # Check for anything else
    * )                         echo "You did not type a letter or a digit"
esac
```

#### 2.3.3. loops

```bash
#!/bin/bash

number=0
while [ "$number" -lt 10 ]; do
    echo "Number = $number"
    number=$((number + 1))
done

number=0
until [ "$number" -ge 10 ]; do
    echo "Number = $number"
    number=$((number + 1))
done

count=0
for i in $(cat ~/.bash_profile); do
    count=$((count + 1))
    echo "Word $count ($i) contains $(echo -n $i | wc -c) characters"
done
```


## Reference

- [Linux Command](http://www.linuxcommand.org/index.php)
