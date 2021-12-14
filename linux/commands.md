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


