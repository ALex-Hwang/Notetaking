# Commands worth digging into

- du
- df
- fdisk
- sort
- grep
- egrep
- fgrep
- sed
- awk
- gzip/gzcat/gunzip
- tar
- zip/unzip
- bzip2
- printenv 
- env
- set
- `bash default variants`
- umask
- fsck




# Tips:
- global variables set in child shell won't affect the one in parent shell; unset is the same.

### login shell process
directories to check for a shell login(bash):
- /etc/profile
- $HOME/.bash_profile
- $HOME/.bashrc
- $HOME/.bash_login
- $HOME/.profile

The last four are customized for different users.

> Some distros use PAM(Pluggable Authentication Module), refer [PAM](http://linux-pam.org)

### user & group
`useradd` default: `/etc/skel`
```shell
$ useradd -D
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=
SKEL=/etc/skel
CREATE_MAIL_SPOOL=no
```

### file system
- ext
- ext2

#### journaling filesystem
| Method | Description | 
| ------ | ----------- |
| Data Mode | Both inode and file data are journaled. Low risk off losing data, But poor performance.|
|Ordered mode | Only inode data is written to the journal, but not removed until file data is successfully written. Good compromise between safety and performance.
| Writeback mode| Only inode data is written to the journal, no control over when the file data is written. Higher risk of losing data, but still better than not using journaling filesystem. | 

- ext3(added the journaling system)
- ext4(compression, encryption, and *block preallocation*)

#### CoW filesystem
> It doesn't need the journal system to recover.
- ZFS
- Btrfs

## Shell Scripts

### if statement
```shell
if command
then
    commands
elif command
then
    commands
else
    commands
fi

# also
if command; then
commands
fi
```
If the exit status of the command is zero (the command completed successfully), the commands listed under the *then* section are executed.

##### using test or [] to test condition
```shell
if test condition
then
    commands
fi

# also

if [ condition ]
then
    commands
fi
```

### string comparison

1. When comparing two strings, use \ to escape the `>`.

```shell
if [ $value1 \> $value2 ]
then
    commands
fi
```

2. Comparison is based to ASCII ordering, while `sort` is based on the system locale.

> `Testing` is greater than `testing` when `sort` is being used, less when `shell string comparison` is used.

### advanced test
1. double parentheses for mathmatical expressions.
2. double square brackets for advanced string handling function.

### case
```shell
case variable in
pattern 1 | pattern 2) commands;;
pattern 3) commands;;
pattern 4) commands;;
*) default commands;;
esac
```

### for
```shell
for var in list
do 
    commands
done
```

**Notice:** After the last iteration, the `$var` variable remains valid throughout the remainder of the shell script.

#### Reading values from a command

```shell
for state in $(cat files)
do
    commands
done
```

#### File Separator

A special environment variable **IFS**,  called the *internal field separator*. By default, the bash shell considers the below as field separators:
- A space
- A tab
- A newline

You can temperarily change it in this way:

```shell
IFS=$'\n'
```

You can also specify more than one IFS, by doing this:

```shell
# In this case, you specify the newline, colon, semicolon, and doube quotation mark as filed separators.
IFS=$'\n':;"
```
#### Reading a directory using wildcards

```shell
for file in /home/helix/*
do
    if [ -d "$file" ]
    then
        commands
    fi
done
```

### C-style commands

```shell
for (( i=1; i <= 10; i++ ))
do
    commands
done
```

### while

**the format**:
```shell
while test command
do
    other commands
done
```

**example**
```shell
var1=10
while [ $var1 -gt 0 ]
do
    echo $var1
    $var1=$[ $var1 - 1 ]
done
```


#### Using multiple test commands
```shell
#!/bin/bash
var1=10
while echo $var1
    [ $var1 -ge 0 ]
do
    echo "This is inside the loop"
    var1 = $[ $var1 - 1 ]
done



$ ./test
10
This is inside the loop
9
This is inside the loop
8
This is inside the loop
7
This is inside the loop
6
This is inside the loop
5
This is inside the loop
4
This is inside the loop
3
This is inside the loop
2
This is inside the loop
1
This is inside the loop
0
This is inside the loop
-1
```

### until

which requires that you specify a test command that normally produces a none zero exit status.

```shell
until test commands
do
    other commands
done
```


### Processing the Output of a Loop
```shell
# redirecting the for output to a file
for (( a = 1; a < 10; a++ ))
do
    echo "The number is $a\\"
done > test.txt
echo "Finished"
```

### Reading Parameters
`$1` `$2` `${11}`, and so on.

#### Reading script name
`$0` or `basename $0`

#### Special Parameters
- `$#`: the number of parameters
- `${!#}`: the last parameter
- `$*`: takes all the parameters as a single word
- `$@`: takes all the parameters as separate words

#### Being shift
Shift the parameters using `shift`:
```shell
count=1
while [ -n "$1" ]
do
    echo "Parameter #$count = $1"
    count=$[ $count + 1 ]
    shift
    # you can also specify the number: n
    # shift 2
done
```

#### getops
search Google for more info about `getops`


### Getting User Input

using `read` command: which assign all data entered into one single variable, or you can specify multiple ones.
```shell
read options varibles
```
```shell
read -p "message" -s -t 5 test"
```
which means: print "message", not printing the data entered, and set a 5s timer.

#### Reading from a File

When no more lines are left in the file, the `read` command exists with a non-zero status.

```shell
#!/usr/bin/bash
count=1
cat test | while read line
do
    echo "Line $count: $line"
    count=$[ $count + 1 ]
done
echo "Finished"
```

### Redirecting
#### both STDOUT and STDERR
`&>`

#### redirecting output in scripts
- temporary redirections
```shell
echo "This is an error" >&2
echo "This is a message"

$ ./test 2> error.test
```

- permanent redirections
```shell
exec 1> output.test
```


