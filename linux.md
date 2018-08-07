`pwd`<br>
`ls -la` - list files/dirs in cwd, one per line, including hidden, . and ..<br>
`du -b <file>` disk usage (apparent size in bytes)<br>
`cat <file>` print file content<br>
`cd <dir>`<br>
`mkdir <new_dir>`<br>
`mkdir -p <new_dir>` - create parent dir(s) if needed<br>
`rmdir <dir_name>`<br>
`mv <src> <dest>`<br>
`cp <src> <dest>`<br>
`rm <file|empty_dir>`<br>
`rm -rf <file|dir>` - recursive, force<br>
`link <filename> <new_hardlink_name>`<br>
`ln -s <filename> <symlink>`<br>
`whatis <glob>`<br>
`whereis <glob>` - related src, bin, man<br>
`which <glob>` - executable file<br>
`echo $PATH | tr ":" "\n" | nl`<br>
`chown <username> <dirpath>`<br>
`chmod +x <filename>` make file executable<br>
```
   user   group  others
   r w x  r w x  r w x

0  - - -
1  - - x
2  - w -
3  - w x
4  r - -
5  r - x
6  r w -
7  r w x

app_mode     0666   rw- rw- rw-
umask        0002   000 000 0-0
app & ~umask __________________
file_mode    0664   rw- rw- r-- 
```

`umask`<br>
`groups`<br>
`groupadd <group>`<br>
`usermod -aG <group> <username>` - add user ($USER for current user) to group<br>
`gpasswd -d <username> <group>` - remove user from group<br>
`gnome-session-quit`<br>

`<command> &` - run the command in the background<br>
`Ctrl + z` - stop current process and bring it to the background<br>
`jobs` - list all running jobs in current session<br>
`fg <job#>` - bring the job to the foreground<br>
`bg <job#>` - continue the job in the background<br>

`tee`<br>
`tail -n0 -f <filename>` - follow appends to the file<br>
`ls *.bson | xargs -I{} mongorestore {}` - restore each .bson

whoami
lsb_release -cs // show release name (xenial, zesty, etc.)
top // list processes
ps -A
pstree
kill <PID>
kill -9 <PID>
pgrep <search_term> // grep PID from ps -A
pkill <proc_name>
killall <proc_name>
renice <new_priority> <PID>
service <service-name> status
service <service-name> start|stop|restart
   
ll /var/lib/mlocate
sudo updatedb
locate *dir/*.ext

time sudo grep -Rl "/nvm" ~ # find files that have '/nvm' string inside in ~ directory

alias
alias echopath='echo $PATH | tr ":" "\n" | nl'
unalias echopath
echo 'alias echopath='\''echo $PATH | tr ":" "\n" | nl'\' >> ~/.bashrc

source 'vars.env' & bash 'scriptname.sh'
(. vars.env & ./scriptname.sh)

curl -O https://fastdl.mongodb.org/linux/$mongo_version$file_extension # write output to file named as remote resource
tar -zxvf $mongo_version$file_extension # tar, extract, verbose (list files), from file
ln $mongo_version/bin/* ~/bin/ # make links to executable files

lspci
lsusb

apt list --installed
apt policy <package> // show installed and available versions
apt depends <package> // show dependencies
apt rdepends <package> // show dependencies recursively
apt install <package>
apt -s install <package>=<version> // simulate install
apt remove <package>
apt purge <package> // remove with config files

diff <file1> <file2>
diff -s // report if files are the same (default is -q, don't report)
diff -c -C <NUM> // context, NUM lines of it (default 3)
diff -u -U <NUM> // unified context
diff -y --left-columnt // two columns
diff -t --tabsize=<NUM=8> // expand tabs in output
diff -e > <script> | echo w>> <script> // make script for ed/ex
ex - <file1> < <script> // apply script to file

cmp <file1> <file2>
cmp -l // verbose, to EOF
cmp -b // print byte as ASCII
cmp -i, --ignore-initial=<SKIP12>:<SKIP2>// skip first bytes (kB=1000, K=1024, MB, M...)
