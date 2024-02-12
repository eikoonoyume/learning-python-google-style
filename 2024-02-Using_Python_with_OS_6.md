# bash scripting
# basic linux commands
# echo = command to prin messages to screen
# cat = command to show contents of files
# ls = command to list contents of directory
# chmod = command to change permissions of file
# mkdir = command to create new directory
# cd = command to change into that directory
mkdir mynewdir
cd mynewdir/
# many commands don't print when they succeed, only if they fail
# pwd = command to print the current working directory

/mynewdir$ pwd
# output: /home/user/mynewdir
# cp = command to copy files
/mynewdir$ cp ../spider.text .
# copying spider.txt located in previous directory to this directory
# .. dot-dot (identify the previous directory) shortcut reverses a parent directory, the previous directory and the absolute path
# . dot shortcut reverses the current directory

# touch = command to create an empty file
/mynewdir$ touch myfile.txt

# ls -l (ls dash l) = command to call ls command to look at contents of directory
/mynewdir$ ls -l
# output: -rw-rw-r-- 1 user user   0 Mai 22 14:22 myfile.txt
# output: -rw-rw-r-- 1 user user 192 Mai 22 14:18 spider.txt

# ls -la(ls dash la) = command to show hidden files which are the ones that start with a dot
/mynewdir$ ls -la
# output: total 12
# output: drwxr-xr-x  2 user user  4096 Mai 22 14:17 .
# output: drwxr-xr-x 56 user user 12288 Mai 22 14:17 ..
# output: -rw-rw-r--  1 user user     0 Mai 22 14:22 myfile.txt
# output: -rw-rw-r--  1 user user   192 Mai 22 14:18 spider.txt

# mv = command to rename or move a file
/mynewdir$ mv myfile.txt emptyfile.txt

# cp = command to copy file
/mynewdir$ cp spider.txt yetanotherfile.txt
# first parameter = old file, second = new file
/mynewdir$ ls -l
# output: total 8
# output: -rw-rw-r-- 1 user user   0 Mai 22 14:22 emptyfile.txt
# output: -rw-rw-r-- 1 user user 192 Mai 22 14:18 spider.txt
# output: -rw-rw-r-- 1 user user 192 Mai 22 14:23 yetanotherfile.txt

# rm = command to delete file
# use * to delete all together
/mynewdir$ rm *
/mynewdir$ ls -l
# output: total 0
# * = placeholder that gets swapped out by the name of all files in directory

# change to previous directory using cd ..
# rmdir = command to delete directory (only works on empty dictionaries!)
/mynewdir$ cd ..
rmdir mynewdir/
ls mynewdir
# output: no such file or directory

# Redirecting Streams

