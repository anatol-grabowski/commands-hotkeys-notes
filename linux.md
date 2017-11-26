pwd
ls -la // list files/dirs in cwd, one per line, including hidden, . and ..
du -b <file> // disk usage (apparent size in bytes)
cat <file> // print file content
cd <dir>
mkdir <new_dir>
mkdir -p <new_dir> // create parent dir(s) if needed
rmdir <dir_name>
mv <src> <dest>
cp <src> <dest>
rm <file|empty_dir>
rm -rf <file|dir> // recursive, force
link <filename> <new_hardlink_name>
ln -s <filename> <symlink>
whatis <glob>
whereis <glob> // related src, bin, man
which <glob> // executable file
echo $PATH | tr ":" "\n" | nl
chmod +x <filename> // make file executable

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

umask
groups
groupadd <group>
usermod -aG <group> <username> // add user ($USER for current user) to group
gpasswd -d <username> <group> // remove user from group
gnome-session-quit

whoami
lsb_release -cs // show release name (xenial, zesty, etc.)
top // list processes
ps -A
pstree
kill <PID>
pgrep <search_term> // grep PID from ps -A
pkill <proc_name>
killall <proc_name>
renice <new_priority> <PID>
service <service-name> status
service <service-name> start|stop|restart

alias
alias echopath='echo $PATH | tr ":" "\n" | nl'
unalias echopath
echo 'alias echopath='\''echo $PATH | tr ":" "\n" | nl'\' >> ~/.bashrc

source 'vars.env' & bash 'scriptname.sh'
(. vars.env & ./scriptname.sh)


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
