# Server Setup Week 5 (10/25/2021)
## Shell Script
shell Script is a program that is written to be run by Unix/Linux Shell. It's contain a series of commands for the shell to execute. As an example you can write a shell script that can help you to identify whether you are a user or a super user.

First make a new file, name it as check.sh. You can use `$vim check.sh`, `$gedit check.sh` or other text editor. Write the following command in check.sh :

```
#!/usr/bin/bash

if [ $UID -eq 0 ];
then
  echo "you are super user"
else
  echo "you are normal user"
fi
```

give execute access to the file `#chmod +x check.sh` then you can run the program `./check.sh`. If you want your program can be run in any working directory you need to make sure that the directory where you put your program is already listed in $PATH. If you type `$echo $PATH` and didn't see the path there, you can follow the step from this website [linuxize.com](https://linuxize.com/post/how-to-add-directory-to-path-in-linux/).

## Linux Environment Path
Below are some common environment variables :
* `HOME` : home directory of current user
* `USER` : show the current logged in user
* `PATH` : directories list of executable command
* `PWD` : show current working directory
* `RANDOM` : used to generate random integers

## Linux Command
### 1). alias

```
$ alias [name[=value]...]
```
use other string to call a command. For example if we type :

```
$ alias ll='ls -l -h'
```

this way in your current terminal if you type `$ ll`, it will execute `ls -l -h`. But this is only a temporary setting, you can add this alias into `~/.bashrc`. While to delete it from setting you can use `$ unalias`

### 2). Make a longer random number

add md5sum to make a longer random number

```
$ echo $RANDOM | md5sum
```

you can set how many number for the output by using `cut -c`

```
echo $RANDOM | md5sum | cut -c 1-9
```

this way the output will be 9 characters

### 3). Show environment variable
To show the environment variable you can execute the following command

```
$ env
```

### 4). Set how many times to ping and its timeout

if you want to try ping once and set its timeout you can write the following code

```
$ ping -c 1 -W 1 8.8.8.8
```

`-c` : will set how many times to ping

`-W` : set the timeout time in second

### 5). test
test is a command that used to evaluates conditional expressions.

```
$ test expression
or
$ [expression]
```

Arguments :

`-e FileName` : check whether the file is exist

`-w FileName` : check whether the file is writeable

`-x FileName` : check whether the file is executable

`-s FileName` : check whether the file is empty

`-r FileName` : check whether the file is readable

`-d FileName` : check whether the file is directory

`-L FileName` : check whether the file is link file

Examples :

```
[root@nubletz tmp]$ b=""
[root@nubletz tmp]$ test -n "$b"
[root@nubletz tmp]$ echo $?
1


[root@nubletz tmp]$ a="123"
[root@nubletz tmp]$ test -n "$a"
[root@nubletz tmp]$ echo $?
0

#Notes : you need to use "..." instead of '...' or else it will always return 0

[root@nubletz tmp]# a=123
[root@nubletz tmp]# test "$a" == "$b"
[root@nubletz tmp]# echo $?
1

[root@nubletz tmp]# test "$a"=="$b"
[root@nubletz tmp]# echo $?
0
[root@nubletz tmp]# echo "$a"=="$b"
123==

#Notes : you need to give space, or else it will cause error
```