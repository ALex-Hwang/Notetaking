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
