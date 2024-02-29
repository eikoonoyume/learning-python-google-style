# Diffing Files
# diff = command to take 2 files/directories and show differences between them
# use diff with rearrange1.py and rearrange2.py
# look at the file
cat rearrange1.py
# output:
#!/usr/bin/env python3
import re
def rearrange_name(name):
    result = re.search(r"^([\w .]*), ([\w .]*)$", name)
    if result == None:
        return name
    return "{} {}".format(result[2], result[1])
user@ubuntu:~$ cat rearrange2.py 
#!/usr/bin/env python3
import re
def rearrange_name(name):
    result = re.search(r"^([\w .-]*), ([\w .-]*)$", name)
    if result == None:
        return name
    return "{} {}".format(result[2], result[1])

# and the other file
cat rearrange2.py 
# output:
#!/usr/bin/env python3
import re
def rearrange_name(name):
    result = re.search(r"^([\w .-]*), ([\w .-]*)$", name)
    if result == None:
        return name
    return "{} {}".format(result[2], result[1])

# use diff
diff rearrange1.py rearrange2.py
# output:
6c6
<     result = re.search(r"^([\w .]*), ([\w .]*)$", name)
---
>     result = re.search(r"^([\w .-]*), ([\w .-]*)$", name)
# < = first line was removed from first file
# > = second line was added to second file
# in other words, the old line got replaced by the new one (?)

# check validations1.py and validations2.py
diff validations1.py validations2.py
# output:
5c5,6
<	assert (type(username) == str), "username must be a string"
--
>	if type(username != str: 
> 	    raise TypeError("username must be a string"
11a13,15
>	    return False
>	# Usernames can't begin with a number
>	if username[0].isnumeric():

# diff splits the changes into two sections
# 5c5,6 section shows a line in the first file was replaced by two different lines in second file
# c in between the numbers means a line was changed
# 11a13,15 section shows three lines that are new in second file
# a in between the numbers means added

# use -u flag to tell diff to show differences in another format
# -u = unified format, shows changed lines together with some context
# uses - minus sign to mark lines that were removed, + plus sing to mark lines that were added

diff -u validations1.py validations2.py
# output:
--- validations1.py	2019-06-06 14:28:49.639209499 +0200
+++ validations2.py	2019-06-06 14:30:48.019360890 +0200
@@ -2,7 +2,8 @@
 
 
 def validate_user(username, minlen):
-    assert type(username) == str, "username must be a string"
+    if type(username) != str:
+        raise TypeError("username must be a string")
     if minlen < 1:
         raise ValueError("minlen must be at least 1")
     
@@ -10,5 +11,8 @@
         return False
     if not username.isalnum():
         return False
+    # Usernames can't begin with a number
+    if username[0].isnumeric():
+        return False
     return True
# can see new if block that's part of a chain of conditionals

# wdiff = highlights the words that have changed in a file instead of working line by line like diff does
# meld, KDiff3, vimdiff = graphical tools that display files side by side and highlight the differences using color

# Applying Changes
# send colleague a diff, make the change clear 
# use a command line like diff-u old_file new_file>change.diff
# > = redirects output of the diff command to the file
# -u flag = include more context which helps person reading the file understand the change
# generated file = diff file (or patch file), includeds all changes between old and new file, plus additional context

# apply changes with a command: patch
# patch = takes file generated by diff, applies changes to original file
cat cpu_usage.py
# output:
#!/usr/bin/env python3
import psutil
def check_cpu_usage(percent):
    usage = psutil.cpu_percent()
    return usage < percent
if not check_cpu_usage(75):
    print("ERROR! CPU is overloaded")
else:
    print("Everything ok")

# psutil module was used to check percentage of CPU that's currently in use
# when load is above threshold (in this case 75%), error message is printed
# when load is under threshold, "Everything's ok" is printed
cat cpu_usage.diff
# output:
--- cpu_usage.py	2019-06-23 08:16:04.666457429 -0700
+++ cpu_usage_fixed.py	2019-06-23 08:15:37.534370071 -0700
@@ -2,7 +2,8 @@
 import psutil
 
 def check_cpu_usage(percent):
-    usage = psutil.cpu_percent()
+    usage = psutil.cpu_percent(1)
+    print("DEBUG: usage: {}".format(usage))
     return usage < percent
 
 if not check_cpu_usage(75):
# added parameter to CPU percent function, added debugging line that prints the value returned by function
# use patch, pass name of file to patch (cpu_usage.py) as first parameter
# provide diff file through standard input, use < symbol to redirect contents of diff file to standard input

patch cpu_usage.py < cpu_usage.diff
# output:
patching file cpu_usage.py

# look at contents of script again
cat cpu_suage.py
# output:
#!/usr/bin/env python3
import psutil
def check_cpu_usage(percent):
    usage = psutil.cpu_percent(1)
    print("DEBUG: usage: {}".format(usage))
    return usage < percent
if not check_cpu_usage(75):
    print("ERROR! CPU is overloaded")
else:
    print("Everything ok")
# CPU percent function is being called with parameter of 1, debugging line is printed

# Practical Application of diff and patch
# goal of script is to check how much disk space is currently used, print error if it's too little space for normal operation
# make a couple of copies of script
# add _original to one copy = keep this file unmodified, use it for comparison
# add _fixed to the other = use to repair our fix
cp disk_usage.py disk_usage_original.py
cp disk_usage.py disk_usage_fixed.py

#!/usr/bin/env python3
import shutil

def check_disk_usage(disk, min_absolute, min_percent):
  """Returns True if there is enough free disk space, false otherwise"""
  du = shutil.disk_usage(disk)
  # calculate the percentage of free space
  percent_free = 100 * du.free / du.total
  # calculate how many free gigabytes
  gigabytes_free = du.free / 2**30
  if percent_free < min_percent of gigabytes_free < min_absolute:
    return False
  return True
# calculate for at least 2 GB and 10% free
if not check_disk_usage("/", 2*2**30, 10):
  print("ERROR: Not enough disk space")
  return 1
print("Everything ok")
return 0

# edit _fixed version
# execute it first
./disk_usage_fixed.py
# error output:
# File "./disk_usage_fixed.py", line 19
    return 1
    ^
SyntaxError: 'return' outside function
# Error says there's a return outside function. Can only return statements inside functions
# option 1: turn current code into function and then call that function from main part of script
# option 2: use sys.exit to make the return number of the exit code of our script
# exit code = code that causes program to exit with the corresponding exit value

#!/usr/bin/env python3

 import shutil
 import sys

 def check_disk_usage(disk, min_absolute, min_percent):
   """Returns True if there is enough free disk space, false otherwise."""
     du = shutil.disk_usage(disk)
     # Calculate the percentage of free space
     percent_free = 100 * du.free/ du.total
     # Calculate how many free gigabytes
     gigabytes_free = du.free / 2**30
     if percent_free < min_percent or gigabytes_free < min_absolute:
       return False
      return True
# Check for at least 2 GB and 10% free
  if not check_disk_usage("/", 2*2**30, 10):
    print("ERROR: Not enough disk space")
    sys.exit(1)
  print("Everything ok")
  sys.exit(0)
# execute
./disk_usage_fixed.py
# output:
ERROR: Not enough disk space
# Error: script is converting to gigabytes twice
# get rid of one of them

#!/usr/bin/env python3

import shutil
import sys

def check_disk_usage(disk, min_absolute, min_percent):
  """Returns True if there is enough free disk space, false otherwise."""
  du = shutil.disk_usage(disk)
  # Calculate the percentage of free space
  percent_free = 100 * du.free / du.total
  # Calculate how many free gigabytes
  gigabytes_free = du.free / 2**30
  if percent_free < min_percent or gigabytes_free < min_absolute:
    return False
  return True
# Check for at least 2 GB and 10% free
if not check_disk_usage("/", 2, 10):
  print("ERROR: Not enough disk space")
  sys.exit(1)
print("Everything ok")
sys.exit(0)
# execute
./disk_usage_fixed.py
# output: Everything ok

# use diff
diff -u disk_usage_original.py disk_usage_fixed.py > disk_usage.diff
cat disk_usage.diff
# output:
--- disk_usage_original.py	2019-06-22 15:13:38.591579963 -0700
+++ disk_usage_fixed.py	2019-06-22 15:41:35.013023839 -0700
@@ -1,6 +1,7 @@
 #!/usr/bin/env python3
 
 import shutil
+import sys
 
 def check_disk_usage(disk, min_absolute, min_percent):
     """Returns True if there is enough free disk space, false otherwise."""
@@ -14,9 +15,9 @@
     return True
 
 # Check for at least 2 GB and 10% free
-if not check_disk_usage("/", 2*2**30, 10):
+if not check_disk_usage("/", 2, 10):
     print("ERROR: Not enough disk space")
-    return 1
+    sys.exit(1)
 
 print("Everything ok")
-return 0
+sys.exit(0)

# use patch
patch disk_usage.py < disk_uage.diff
patching file disk_usage.py
# output:
patching file disk_usage.py
# execute
./disk_usage.py
# output: Everything ok

# Git
# set values of user.email and user.name
git config --global user.email "me@example.com"
git config --global user.name "My name"
# --global = states we want to set this value for all git repositories that we'd use

# git init = create git repository from scratch
# git clone = make a copy of a repository that alreadye exists
# ls -la = lists files that start with a . dot
# ls -l .git = looks inside the directory that ends with .git to see contents

mkdir checks
cd checks
git init
# output: Initialized empty Git repository in /home/user/checks/.git/

ls -la
# output:
total 12
drwxrwxr-x  3 user user 4096 Jul  9 18:16 .
drwxr-xr-x 18 user user 4096 Jul  9 18:16 ..
drwxrwxr-x  7 user user 4096 Jul  9 18:16 .git
user@ubuntu:~/checks$ ls -l .git/
total 32
drwxrwxr-x 2 user user 4096 Jul  9 18:16 branches
-rw-rw-r-- 1 user user   92 Jul  9 18:16 config
-rw-rw-r-- 1 user user   73 Jul  9 18:16 description
-rw-rw-r-- 1 user user   23 Jul  9 18:16 HEAD
drwxrwxr-x 2 user user 4096 Jul  9 18:16 hooks
drwxrwxr-x 2 user user 4096 Jul  9 18:16 info
drwxrwxr-x 4 user user 4096 Jul  9 18:16 objects
drwxrwxr-x 4 user user 4096 Jul  9 18:16 refs

ls -l
# output:
total 4
-rw-rw-r-- 1 user user 657 Jul  9 18:26 disk_usage.py

ls -l .git/
# output:
total 32
drwxrwxr-x 2 user user 4096 Jul  9 18:16 branches
-rw-rw-r-- 1 user user   92 Jul  9 18:16 config
-rw-rw-r-- 1 user user   73 Jul  9 18:16 description
-rw-rw-r-- 1 user user   23 Jul  9 18:16 HEAD
drwxrwxr-x 2 user user 4096 Jul  9 18:16 hooks
drwxrwxr-x 2 user user 4096 Jul  9 18:16 info
drwxrwxr-x 4 user user 4096 Jul  9 18:16 objects
drwxrwxr-x 4 user user 4096 Jul  9 18:16 refs

# working tree = area outside of the git directory, current version of your project (like a workbench/sandbox)
# contains all files currently tracked by Git and any new files not yet added

# copy disk_usage.py file to empty working tree
cp ../disk_usage.py .
ls -l
# output:
4
-rw-rw-r-- 1 user user 657 Jul  9 18:26 disk_usage.py
# currently untracked by Git
# add to make Git track
# git add = pass file as parameter to add to project

# staging area = aka index, file maintained by Git that contains all info about what files and changes are going to go into next command
# git status = get info about current working tree and pending changes
git add disk_usage.py
git status
# output:
On branch master
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   disk_usage.py
# new file is marked "to be committed", meaning our change is currently in staging area. 

# git commit = commit addition into .git directory
git commit
# output:
 GNU nano 3.2         /home/user/checks/.git/COMMIT_EDITMSG                    
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   disk_usage.py

# git commit = saves our changes, opens text editor where we can enter a commit message 

# Tracking Files
# check contents of current working tree using ls -l
# ls -l = checks contents of working tree
# git status to check current status of files
cd checks
ls -l
# output:
total 4
-rw-r--r-- 1 user user 657 Jul  9 12:52 disk_usage.py
user@ubuntu:~/checks$ git status
On branch master
nothing to commit, working tree clean

# nothing to commit, working tree is clean, now modify a file
# add periods at end of message, once changed, call git status
git status
# output:
On branch master
nothing to commit, working on clean tree

# git add = pass disk_usage.py file as parameter, stage the change for commit
atom disk_usage.py
git status
# output:
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   disk_usage.py
# git commit -m = pass commit message using -m flag
git commit -m 'Add periods to the end of sentences.'
# output:
[master ae8d19c] Add periods to the end of sentences.
 1 file changed, 2 insertions(+), 2 deletions(-)

# last status check
git status
# output:
On branch master
nothing to commit, working tree clean
# starting over with no changes to commit

# Basic Git Workflow
# mkdir = create a directory called scripts
# change into it, intialize empty Git repository init
mkdir scripts
cd scripts
git init
# output: Initialized empty Git repository in /home/user/scripts/.git/
# use git config -l = check out current configuration 
git config -l
# output:
user.email=me@example.com
user.name=My name
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
# add empty main fuction for now

#!/usr/bin/env python3

def main():
  pass
main()
# execute, check status
chmod +x all_checks.py
git status
# output:
On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	all_checks.py
nothing added to commit but untracked files present (use "git add" to track)

# git add to move new file from untracked to stage status
# initiate commit of staged files using git commit
# git commit without parameters will launch a text editor
git add all_checks.py
git commit

Create an empty all_checks.
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   all_checks.py
#

# save and exit
# add function check_reboot to check if the computer is pending a reboot
# check if run/reboot-required files exists
#!/usr/bin/env python3

import os

def check_reboot():
  """Returns True if the computer has a pending reboot."""
  return os.path.exists("/run/rebook-required")

def main():
  pass
  
main()

# check status
git status
# output:
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
	modified:   all_checks.py
no changes added to commit (use "git add" and/or "git commit -a")

# git add to stage our changes
git add all_checks.py
git status
# output:
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
	modified:   all_checks.py

# call git commit -m, pass the commit message
git commit -m 'Add a check_reboot function'
# output:
[master d8e139c] Add a check_reboot function
 1 file changed, 6 insertions(+)


# Example of Good Commit Message
cat example_commit.txt
# output:
Provide a good commit message example


The purpose of this commit is to provide an example of a hand-crafted,
artisanal commit message. The first line is a short, approximately 50-character summary, followed by an empty line. The subsequent paragraphs are jam-packed with descriptive information about the change, but each line is kept under 72 characters in length.


If even more information is needed to explain the change, more paragraphs can be added after blank lines, with links to issues, tickets, or bugs. Remember that future you will thank current you for your thoughtfulness and foresight!


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Changes to be committed:
# new file:   super_script.py
# new file:   cool_config.txt
#


# check git log for scripts directory before
cd scripts
git log
# output:
commit d8e139cc4f7dcd13b75cff67cfb68527e24c59c5 (HEAD -> master)
Author: My name <me@example.com>
Date:   Thu Jul 11 17:19:32 2019 +0200
    Add a check_reboot function
commit 6cfc29966acda8213fcd8ac2735b31f3fdbc6c53
Author: My name <me@example.com>
Date:   Thu Jul 11 12:08:46 2019 +0200
    Create and empty all_checks.py
# beside word "commit" = identifer

# Qwiklabs: Intro to Git

# get fresh index of packages
sudo apt update
# install Git
sudo apt install git

# prompt: click Y

# check version
git --version

# create directory
mkdir my-git-repo
# navigate to directory
cd my-git-repo
# initialize new repository
git init

# configure name and user email
git config --global user.name "Name"
git config --global user.email "user@example.com"

# create text file README, use nano editor
nano README
# type following text, or any other text
This is my first repository.
# ctrl -o, Enter, ctrl -x to save
# check status
git status
# output:
On branch master


No commits yet


Untracked files:
  (use "git add <file>..." to include in what will be committed)
	
    README


nothing added to commit but untracked files present (use "git add" to track)

# README = untracked currently
# add file to staging area
git add README
# view status
git status 
# output:
On branch master


No commits yet


Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	
    new file:   README


# commit changes
git commit
# enter commit message
This is my first commit!
# ctrl -o, Enter, ctrl -x to save
# re-edit file again
nano README
# Add following line or any other
A repository is a location where all the files of a particular project are stored.
# ctrl -o, Enter, ctrl -x to save
# check status
git status
# output:
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	
    modified:   README


no changes added to commit (use "git add" and/or "git commit -a")

# view changes
git diff README
# add changes to staging area
git add README
# view status
git status
# commit file with message
git commit -m "This is my second commit."
# view log
git log