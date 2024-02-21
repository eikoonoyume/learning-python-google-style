# Skipping the Staging Area
# git commit -a = automatically stages every file that's tracked and modified before doing the commit, skipping git add step
# doesn't work on new files because they are untracked
# modify main function
# call check_reboot() function
# print message if reboot is pending
# exit program with exit status 1
cd scripts
atom all_checks.py

#!/usr/bin/env python3

import os
import sys

def check_reboot():
  """Returns True if the computer has a pending reboot."""
  return os.path.exists("/run/reboot-required")
def main():
  if check_reboot():
    print("Pending Reboot.")
    sys.exit(1)
main()
# use -a and -m flag to add commit message directly
git commit -a -m "Call check_reboot from main, exit with 1 on error"
# output: 
[master 033f27a] Call check_reboot from main, exit with 1 on error
 1 file changed, 4 insertions(+), 1 deletion(-)

# -m = can only write short meesages, best for truly small changes that don't need context or explanation
# -a = can't add any other changes before creating commit

git log
# output:
commit 033f27a8196987d61c4fd42930f2148b23434a03 (HEAD -> master)
Author: My name <me@example.com>
Date:   Mon Jul 15 14:39:18 2019 +0200
    Call check_reboot from main, exit with 1 on error
commit cc1acbf10fdea6cc07ebf827697666b6a35b0f36
Author: My name <me@example.com>
Date:   Thu Jul 11 17:19:32 2019 +0200
    Add a check_reboot function
commit 6cfc29966acda8213fcd8ac2735b31f3fdbc6c53
Author: My name <me@example.com>
Date:   Thu Jul 11 12:08:46 2019 +0200
    Create and empty all_checks.py
# latest commit goes on top, becomes head

# -p flag = look at actual lines that changed in each commit, p = patch
git log -p
# output:
commit 033f27a8196987d61c4fd42930f2148b23434a03 (HEAD -> master)
Author: My name <me@example.com>
Date:   Mon Jul 15 14:39:18 2019 +0200
    Call check_reboot from main, exit with 1 on error
diff --git a/all_checks.py b/all_checks.py
index 340f1f7..710266a 100644
--- a/all_checks.py
+++ b/all_checks.py
@@ -1,12 +1,15 @@
 #!/usr/bin/env python3
 
 import os
+import sys
 
 def check_reboot():
     """Returns True if the computer has a pending reboot."""
     return os.path.exists("/run/reboot-required")
(...)
# format = diff -u output, + = added lines, - = removed lines, use page up/down or arrow keys to scroll

# use git show command if you don't want to scroll
# git show <commit ID ex. 033f27a8196987d61c4fd42930f2148b23434a03> to display info about this commit

# exit using q

git log
# output:
commit 033f27a8196987d61c4fd42930f2148b23434a03 (HEAD -> master)
Author: My name <me@example.com>
Date:   Mon Jul 15 14:39:18 2019 +0200
    Call check_reboot from main, exit with 1 on error
commit cc1acbf10fdea6cc07ebf827697666b6a35b0f36
Author: My name <me@example.com>
Date:   Thu Jul 11 17:19:32 2019 +0200
    Add a check_reboot function
(...)
user@ubuntu:~/scripts$ git show cc1acbf10fdea6cc07ebf827697666b6a35b0f36
commit cc1acbf10fdea6cc07ebf827697666b6a35b0f36
Author: My name <me@example.com>
Date:   Thu Jul 11 17:19:32 2019 +0200
    Add a check_reboot function
diff --git a/all_checks.py b/all_checks.py
index c0d03b3..340f1f7 100644
--- a/all_checks.py
+++ b/all_checks.py
@@ -1,5 +1,11 @@
 #!/usr/bin/env python3
 
+import os
+
+def check_reboot():
+    """Returns True if the computer has a pending reboot."""
+    return os.path.exists("/run/reboot-required")
+
 def main():
     Pass

# --stat = flag that will cause git log to show some stats about changes in commit
# examples: which files were changd, how many lines added/removed

git log --stat
# output:
commit 033f27a8196987d61c4fd42930f2148b23434a03 (HEAD -> master)
Author: My name <me@example.com>
Date:   Mon Jul 15 14:39:18 2019 +0200
    Call check_reboot from main, exit with 1 on error
 all_checks.py | 5 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)
(...)

# gif diff = command that helps keep track of what's changed before commit
# add another message to say when check is successful "everything ok"
# exit with 0
atom all_checks.py
#!/usr/bin/env python3

import os
import sys

def check_reboot():
  """Returns True if the computer has a pending reboot."""
  return os.path.exists("/run/reboot-required")
def main():
  if check_reboot():
    print("Pending Reboot.")
    sys.exit(1)
  print"Everything ok.")
  sys.exit(0)
main()

git diff
# output:
diff --git a/all_checks.py b/all_checks.py
index 710266a..fdc4476 100644
--- a/all_checks.py
+++ b/all_checks.py
@@ -12,4 +12,7 @@ def main():
         print("Pending Reboot.")
         sys.exit(1)
 
+    print("Everything ok.")
+    sys.exit(0)
+
 main()

# git add -p = command to review changes before adding
# will ask if you want to stage the change or not
git add -p
# output:
diff --git a/all_checks.py b/all_checks.py
index 710266a..fdc4476 100644
--- a/all_checks.py
+++ b/all_checks.py
@@ -12,4 +12,7 @@ def main():
         print("Pending Reboot.")
         sys.exit(1)
 
+    print("Everything ok.")
+    sys.exit(0)
+
 main()
Stage this hunk [y,n,q,a,d,e,?]? y
user@ubuntu:~/scripts$ 

# call git diff again, it won't show any differences since git diff only shows unstaged changes by default
# call git diff --staged = see changes that are staged but not committed
git diff
git diff --staged
# output:
diff --git a/all_checks.py b/all_checks.py
index 710266a..fdc4476 100644
--- a/all_checks.py
+++ b/all_checks.py
@@ -12,4 +12,7 @@ def main():
         print("Pending Reboot.")
         sys.exit(1)
 
+    print("Everything ok.")
+    sys.exit(0)
+
 main()


git commit -m 'Add a message when everything is ok'
Code output:
[master 49d610b] Add a message when everything is ok
 1 file changed, 3 insertions(+)


# Deleting and Renaming Files
# git rm = command to remove files from repository
# write commit message for why you deleted them
# look at contents with ls
# delete file with git rm
# check contents with ls
# check status with git status
cd checks/
ls -l
# output:
total 8
-rw-rw-r-- 1 user user 659 Jul  9 19:28 disk_usage.py
-rw-rw-r-- 1 user user 659 Jul 15 21:43 processes.py

git rm process.py
# output:
rm 'process.py'

ls -l
# output:
total 4
-rw-rw-r-- 1 user user 659 Jul  9 19:28 disk_usage.py

git status
# output:
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        deleted:    process.py

# git commit -m to say why you deleted
git commit -m 'Delete unneeded processes file'
# output:
[master 9939311] Delete unneeded processes file
 1 file changed, 24 deletions(-)
 delete mode 100644 process.py
# 24 lines were deleted

# git mv = command to rename files in repository
git mv disk_usage.py check_free_space.py
git status
# output:
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        renamed:    disk_usage.py -> check_free_space.py
# commit
git commit -m 'New name for disk_usage.py'
# output:
[master 7d7167b] New name for disk_usage.py
 1 file changed, 0 insertions(+), 0 deletions(-)

# can use git mv to move files between directories

# long lists of untracked files, files automatically generated, artifacts generated by OS all create noise that we want to ignore as we get output from git status
# gitignore file = ignores extra noise
# specify rules in this file to tell git which files to skip for current repo
# if working on OSX computer, ignore .DS_store file which is automatically generated by OS
echo .DS_STORE > gitignore
ls -la
# output:
total 20
drwxrwxr-x  3 user user 4096 Jul 15 22:15 .
drwxr-xr-x 19 user user 4096 Jul 15 16:37 ..
-rw-rw-r--  1 user user  659 Jul  9 19:28 check_free_space.py
drwxrwxr-x  8 user user 4096 Jul 15 21:52 .git
-rw-rw-r--  1 user user   10 Jul 15 22:15 .gitignore
# dot for .DS_STORE indicates the file/directory is hidden
# use ls -la to see all files, even if hidden

# the added gitignore file must be tracked then committed
git add .gitignore
git commit -m 'Add a gitignore file, ignoring .DS_STORE files'
# output:
[master abb0632] Add a gitignore file, ignoring .DS_STORE files
 1 file changed, 1 insertion(+)
 create mode 100644 .gitignore

# Undoing Changes Before Committing
# use git checkout <name of file to revert> = command to revert changes to an earlier committed state
# edit checks.py script
# remove check_reboot function
# save and go back to command line
cd scripts
atom all_checks.py

# will change to
#!/usr/bin/env python3
import os
import sys

def main():
  if check_reboot():
    print("Pending Reboot.")
    sys.exit(1)
  print("Everything ok.")
  sys.exit(0)
main()
# run script
./all_checks.py
# output:
Traceback (most recent call last):
  File "all_checks.py", line 14, in <module>
    main()
  File "all_checks.py", line 7, in main
    if check_reboot():
NameError: name 'check_reboot' is not defined
# script broken, use git status

git status
# output:
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   all_checks.py
# file = modified, changes not staged  yet
# git status gives tips like "git add" to update/stage, "git checkout" to discard
# git checkout = check out original file from latest storage snapshot
git checkout all_checks.py
git status
# output:
On branch master
nothing to commit, working tree clean
user@ubuntu:~/scripts$ ./all_checks.py 
Everything ok.


# typo exists in "return os.path.exist(". Fix to "return os.path.exists("
# run
./all_checks.py
# output: Everything ok.

# use git checkout to revert changes to modify files before they get staged, restores file to the latest storage snapshot (can be either committed or staged) so you can restore to earlier stage version if you've made changes to file after staging

# use -p flag to check out individual changes instead of whole file
# will ask you change by change if you want to go back to previous snapshot or not

# use git reset command to unstage changes that we didn't really want to commit

# try to debug a problem in script, create a temp file with output of script
# use git add * = add all unstaged changes in working tree
# use git status to check
./all_checks.py > output.txt
git add *
git status
# output:
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        new file:   output.txt
# tells us how to unstage the file
# head = current checked out snapshot, reset to the current snapshot (head)
git reset HEAD output.txt
git status
# output:
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Output.txt

# file = untracked in working tree, no longer staged
# reset is like the counterpart to "add"
# add = add changes to staging area
# reset = remove changes from staging area
# use git reset -p = makes git ask you which specific changes you want to reset
git commit -m "it should be os.path.exists"


# Amending Commits
# example issues: error in recent commit, commit isn't descriptive enough
# use git commit --amend = git takes whatever is currently in staging area, runs git commit workflow to overwrite previous commit

# create two new files using touch command
# list contents of directory using ls
# commit saying we added two files
cd scripts/
touch auto-update.py
touch gather-information.sh
ls -1
# output:
total 8
-rwxrwxr-x 1 user user 319 Jul 16 17:56 all_checks.py
-rw-rw-r-- 1 user user   0 Jul 16 20:19 auto-update.py
-rw-rw-r-- 1 user user   0 Jul 16 20:19 gather-information.sh
-rw-rw-r-- 1 user user  15 Jul 16 18:03 output.txt
user@ubuntu:~/scripts$ git add auto-update.py 
user@ubuntu:~/scripts$ git commit -m 'Add two new scripts'
[master 9c78761] Add two new scripts
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 auto-update.py

 git add auto-update.py
 git commit -m 'Add two new scripts'
 # output:
 [master 919d809] Add two new scripts
1 file changed, 0 insertions(+), 0 deletions(-)
Create mode 100644 auto-update.py
# this says only one file was added, but commit message says we added two
# fix by adding missing file, amend commit
git add gather-information.sh
git commit --amend
# editor opens to show commit message and stats about commit
Add two new scripts.

# Please enter the commit message for your changes. Line starting
#with '#' will be ignored, and an empty message aborts the commit.
#
#Date: Mon Jan 6 08:28:17 2020 -0800
#
# On branch master
# Changes to be committed:
#   new file: auto-update.py
#   new file: gather-information.sh
#
# Untracked files:
#   output.txt


# Add a line of description about the intended purpose of each file

Add two new scripts.

gather-information.sh will collect information in case of errors.
auto-update.py will run daily to update computers automatically.

# Please enter the commit message for your changes. Line starting
#with '#' will be ignored, and an empty message aborts the commit.
#
#Date: Mon Jan 6 08:28:17 2020 -0800
#
# On branch master
# Changes to be committed:
#   new file: auto-update.py
#   new file: gather-information.sh
#
# Untracked files:
#   output.txt


# git commit --amend = can update previous commit message with no changes in staging area

# git commit --amen = only fixes local commits, not public commits (public/shared repository)
# --amend rewrites git history, removing previous commit and replacing with amended one, which leads to confusion with other people

# Rollbacks
# rollback is returning to previous version 
# use git revert = command to rollback commits by creating a commit that contains the inverse of all changes made in the bad commit in order to cancel them out (this is not a simple "redo")
# pro: history of commits in project remains consistent, leaving a record of exactly what happened

# add faulty commit to example repo
# save the code to script, commit
cd scripts
atom all_checks.py

#!/usr/bin/env python3

import os
import sys

def check_reboot():
  """Returns True if the computer has a pending reboot."""
  return os.path.exists("/run/reboot-required")
def main():
  if check_reboot():
    print("Pending Reboot.")
    sys.exit(1)
  if disk_full():
    print("Disk Full.")
    sys.exit(1)
  print("Everything ok.")
  sys.exit(0)
main()

git commit -a -m 'Add call to disk_full function'
# output:
[master ec61497] Add call to disk_full function
 1 file changed, 4 insertions(+)
# code is now committed, but we didn't test this code which is bad because it's broken
# run
./all_checks.py
# output:
Traceback (most recent call last):
  File "./all_checks.py", line 22, in <module>
    main()
  File "./all_checks.py", line 15, in main
    if disk_full():
NameError: name 'disk_full' is not defined

# used a function that's not defined, ROLLBACK, get rid of the faulty code by using git revert HEAD
git revert HEAD
# output:
Revert "Add call to disk_full function"
Reason for rollback: The disk_full function is undefined.
This reverts commit ec614976e1665b40134d2c01921f9b0fbf89d1e2.
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Changes to be committed:
#       modified:   all_checks.py
#
# Untracked files:
#       output.txt
#
# first line = reverting commit that was "Add call to disk full function'
# extra description includes commit ID
# prompt to add an explanation for why we're doing rollback
# explain that the reason for rollback is the code was calling a function that wasn't defined
# exit, save
# git revert output and git commit output is same because git revert creates a commit, can be seen in log

git revert HEAD
# output
[master 91c4968] Revert "Add call to disk_full function"
 1 file changed, 4 deletions(-)

# check last two entries of log with -p -2 as parameters
git log -p -2
# output:
commit 91c4968ebd80de900d71b9bc3f332f53149ac57d (HEAD -> master)
Author: My name <me@example.com>
Date:   Tue Jul 16 21:43:18 2019 +0200
    Revert "Add call to disk_full function"
    
    Reason for rollback: The disk_full function is undefined.
    
    This reverts commit ec614976e1665b40134d2c01921f9b0fbf89d1e2.
diff --git a/all_checks.py b/all_checks.py
index 21da366..fdc4476 100755
--- a/all_checks.py
+++ b/all_checks.py
@@ -12,10 +12,6 @@ def main():
         print("Pending Reboot.")
         sys.exit(1)
 
-    if disk_full():
-        print("Disk Full.")
-        sys.exit(1)
-
     print("Everything ok.")
     sys.exit(0)
# -p = lets us see patch created by commit
# -2 = limits output to the last two entries
# revert had removed the lines (the ones with the minus sign) that were added in previous commit

# Identifying Commit
# to revert further back in time, you'll need specific commit, use commit ID
# commit ID = 40 character long string, jumble of letters and numbers = hash, calculated by algorithm SHA1
cd checks
git log -1
# output:
commit abb063210c1f011b0d6470a4c5f1d8f672edd3ef (HEAD -> master)
Author: My name <me@example.com>
Date:   Mon Jul 15 22:20:45 2019 +0200
    Add a gitignore file, ignoring .DS_STORE files
# rare occurrence: two different commits with same has = collision!!

# amend a commit leads to change in commit ID
# IMPORTANT: do NOT use --amend on public commits

# use git log -2 = look at last 2 entries in repo
# use git show = look at specific commit
git log -2
# output:
commit abb063210c1f011b0d6470a4c5f1d8f672edd3ef (HEAD -> master)
Author: My name <me@example.com>
Date:   Mon Jul 15 22:20:45 2019 +0200
    Add a gitignore file, ignoring .DS_STORE files
commit 7d7167b2db44abf8cf014230f9b9708786e41c2a
Author: My name <me@example.com>
Date:   Mon Jul 15 21:52:59 2019 +0200
    New name for disk_usage.py


git show 30e70712882267ca2dd749acfa02ea3aacfd0b24
Code output:
Author: My name <me@example.com>
Date:   Mon Jul 15 21:52:59 2019 +0200
    New name for disk_usage.py
diff --git a/disk_usage.py b/check_free_space.py
similarity index 100%
rename from disk_usage.py
rename to check_free_space.py


# commit ID = long, you can provide usually first 4-8 characters only and that should bring up a match
git show 30e7
Code output:
commit 7d7167b2db44abf8cf014230f9b9708786e41c2a
Author: My name <me@example.com>
Date:   Mon Jul 15 21:52:59 2019 +0200
    New name for disk_usage.py
diff --git a/disk_usage.py b/check_free_space.py
similarity index 100%
rename from disk_usage.py
rename to check_free_space.py


# use git revert command with commit ID
# add reason for rollback in editor ("the previous name was actually better")
git revert 30e7
Code output:
Revert "New name for disk_usage.py"
Rollback reason: the previous name was actually better.
This reverts commit 7d7167b2db44abf8cf014230f9b9708786e41c2a.
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Changes to be committed:
#       renamed:    check_free_space.py -> disk_usage.py
#

# save and exit, Git generates a new commit with new ID
git revert 7d71
Code output:
[master 80b2dac] Revert "New name for disk_usage.py"
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename check_free_space.py => disk_usage.py (100%)

# use git show to look at it
mple.com>
Date:   Wed Jul 17 00:02:39 2019 +0200
    Revert "New name for disk_usage.py"
    
    Rollback reason: the previous name was actually better.
    
    This reverts commit 7d7167b2db44abf8cf014230f9b9708786e41c2a.
diff --git a/check_free_space.py b/disk_usage.py
similarity index 100%
rename from check_free_space.py
rename to disk_usage.py


# Branch
# branch = pointer to particular commit
# master = default branch Git creates for you when new repository is initialized
# master = commonly used to represent known good state of project
# create separate branch to develop features, try something new without messing up current working state
# merge back into master branch when you've got something you like 
# or discard changes without messing with master


# Creating New Branches
# use git branch = list, create, delete, manipulate branches
# git branch alone shows you a list of all branches in repository
cd checks/
git branch
# output:
* master
# empty list
# use git branch <name of new branch> = create branch
# list branches again with git branch
git branch new-feature
git branch
Code output:
* master
  New-feature
# * = still on master branch
# use git checkout = switch to new branch
# use git checkout = restore modified file back to latest commit (git checkout can check out latest snapshots for both FILES and BRANCHES)
git checkout new-feature
Code output:
Switched to branch 'new-feature'
git branch
Code output:
  master
* New-feature
# * is now with New-feature

# use git checkout -b <new branch name> = shortcut creating a branch and switching immediately to is (because this task is so common)
git checkout -b even-better-feature
# output: Switched to a new branch 'even-better-feature'

# save file, commit to current branch
atom free_memory.py 
git add free_memory.py
git commit -m ‘Add an empty free_memory.py’
# output:
[even-better-feature 4361880] Add an empty free_memory.py
1 file changed, 6 insertions(+)
create mode 100644 free_memory.py

# check last two entries in log
git log -2
Commit <commit ID> (HEAD -> even-better-feature)
Author: My name <me@example.com>
Date:    Mon Jan 6 09:47:07 2020 -0800
	Add an empty free_memory.py
Commit <commit ID> (new-feature, master)
Author: My name <me@example.com>
Date:    Mon Jan 6 09:15:58 2020 -0800
	Revert “New name for disk_usage.py”
	Rollback reason: the previous name was actually better :)

# Working with Branches
# use git status and ls -l to check current status of repository
cd checks
git status
# output:
On branch even-better-feature
nothing to commit, working tree clean

ls -l
# output:
total 8
-rw-rw-r-- 1 user user 659 Jul 17 00:02 disk_usage.py
-rw-rw-r-- 1 user user  53 Jul 17 15:33 free_memory.py


# use git checkout master = change back to master branch
# list latest two commits there
git checkout master
git log -2
# output:
Switched to branch 'master'
user@ubuntu:~/checks$ git log -2
commit 80b2dacef4b567196e61651064f03089c5e70b5e (HEAD -> master, new-feature)
Author: My name <me@example.com>
Date:   Wed Jul 17 00:02:39 2019 +0200
    Revert "New name for disk_usage.py"
    
    Rollback reason: the previous name was actually better.
    
    This reverts commit 7d7167b2db44abf8cf014230f9b9708786e41c2a.
commit abb063210c1f011b0d6470a4c5f1d8f672edd3ef
Author: My name <me@example.com>
Date:   Mon Jul 15 22:20:45 2019 +0200
    Add a gitignore file, ignoring .DS_STORE files
# HEAD went from pointing to latest commit in even-better-feature branch to most recent commit of master branch
# look at current contents of directory
ls -l
# output:
total 4
-rw-rw-r-- 1 user user 659 Jul 17 00:02 disk_usage.py
# free_memory.py isn't there because workign directory and commit history will change to reflect snapshot of project in current branch, free_memory.py is in the other branch

# git branch -d = delete a branch we don't need
git branch
Code output:
  even-better-feature
* master
  new-feature

git branch -d new-feature 
Code output:
Deleted branch new-feature (was 80b2dac).

# check that new-feature is gone
git branch
Code output:
  even-better-feature
* master

# if there are changes in branch we want to delete that haven't been merged into master branch, git gives us error
git branch -d even-better
Code output:
error: The branch 'even-better-feature' is not fully merged.
If you are sure you want to delete it, run 'git branch -D even-better-feature'.


# Merging
# merging = combining branch data and history together
# use git merge = takes independent snapshots and history of one branch and tangles them in another branch

# check what branch we're in
# use git merge even-better-feature to merge even-better-feature branch into master branch
git branch
Code output: 
   Even-better-feature
* master



git merge even-better-feature 
Code output: 
Updating 7d1de19..4361880
Fast-forward
  free-memory.py | 6 ++++++
  1 file changed, 6 insertions (+)
  Create mode 100644 free_memory.py

# check log
git log
Code output: 
Switched to branch 'master'
commit 436188012f633b773eb6034fc02051e34da05134 (HEAD -> master, even-better-feature)
Author: My name <me@example.com>
Date:   Mon Jan 6 09:47:07 2020 -0800 
     Add an empty free_memory.py
commit 7d7167b2db44abf8cf014230f9b9708786e41c2a
Author: My name <me@example.com>
Date:   Mon Jan 6 09:15:58 2020 -0800 
    Revert "New name for disk_usage.py"
    
    Rollback reason: the previous name was actually better :)
    
    This reverts commit 7d7167b2db44abf8cf014230f9b9708786e41c2a.
(...)

# even-better-feautre and master branches are both pointing at same commit

# Git has two different algorithms to perform merge
# fast-forward merge = occurs when all commits in check out branch are also in branch that's being merged, commit history of both branches doesn't diverge, Git just needs to update pointers of branches to same commit, no actual merging

# three-way merge = when history of merging branches has diverged in some way and there's no linear path to combine with fast-forward (usually when a commit is made on one branch after the point when both branches split), Git will tie the branch histories together with new commit and merge at the two branch tips with the most recent common ancestor before divergence

# merge conflict = changes were made on same part of the same file, Git doesn't know how to merge

# edit free_memory.py, replace pass statement with comment about what main function should do

atom free_memory.py
#!/usr/bin/env python3

def main():
  """Check if there's enough free memory in the computer"""

main()
# commit to master branch
git commit -a -m 'Add comment to main()'
# output:
[master fe2fc5b] Add comment to main()
  1 file changed, 2 insertions(+), 2 deletions(-)
# make a change in the same place, replace call to pass with call to print "Everything ok"
git checkout even-better-feature
# output:
Switched to branch 'even-better-feature'

atom free_memory.py
#!/usr/bin/env python3

def main():
  print("Everything ok.")
main()
# save, commit to this branch
git commit -a -m 'Print everything ok'
# output:
[even-better-feature 6a6de99] Print everything ok
1 file changed, 2 insertions(+), 2 deletions(-)

# check out master branch again
git checkout master
Code output: 
Switched to branch 'master'

# merge even-better-feature back to master
git merge even-better-feature 
Code output: 
Auto-merging free_memory.py
CONFLICT (content): Merge conflict in free_memory.py
Automatic merge failed; fix conflicts and then commit the result.

# Git doesn't know how to do this
# use git status
git status
Code output: 
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)
Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   free_memory.py
no changes added to commit (use "git add" and/or "git commit -a")
# we have files that are currently unmerged
# fix conflicts or abort merge if it's a mistake
# run git add on each unmerged file to mark that conflicts have been resolved
atom free_memory.py
#!/usr/bin/env python3
def main():
<<<<<<< HEAD
    """Checks if there's enough free memory in the computer."""
=======
    print("Everything ok.")
>>>>>>> even-better-feature


main()
# unmerged content of file at HEAD. HEAD points to master
# unmerged content of file in even-better-feature is the call to print function
# keep both statements and delete merger markers
#!/usr/bin/env python3
  
def main():
    """Checks if there's enough free memory in the computer."""
    print("Everything ok.")


main()
# use git add to mark as resolved
# use git status
git add free_memory.py
git status
Code output: 
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
Changes to be committed:
        modified:   free_memory.py
# use git commit to wrap merge up
git commit
Code output: 
Merge branch 'even-better-feature'
Kept lines from both branches
# Conflicts:
#       free_memory.py
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#       .git/MERGE_HEAD
# and try again.
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#       modified:   free_memory.py


# see commit history
# --graph = seeing commits as a graph
# --oneline = only see one line per commit
git log --graph --oneline
Code output: 
*   8cb5e62 (HEAD -> master) Merge branch 'even-better-feature'
|\  
| * ca6de99 (even-better-feature) Print everything ok
* | fe2fc5b Add comment to main()
|/  
* 4361880 Add an empty free_memory.py
* 7d1de19 Revert "New name for disk_usage.py"
* bb9bd78 Add a gitignore file, ignoring .DS_STORE files
* 30e7071 New name for disk_usage.py
* 0d5a271 Delete unneeded processes file
* 5aada26 Adding file to delete it later
* cfb2b8e Add periods to the end of sentences.
* 21e6a1a Add new disk_usage check.

# master is pointing to merge commit but even-better-feature is still pointing to previous one

# use git merge --abort = throw the merge away and start over, escape hatch, will stop merge and reset files back to previous commit before merge

# Qwiklabs: Merge branches in Git
# navigate
cd ~/food-scripts
# list files
ls
# favorite_foods.log food_count.py food_question.py
# view each file
cat favorite_foods.log
# execute script for food_count.py
./food_count.py
# run to see food_question.py output
./food_question.py
# error message

# use git status = displays paths that have differences between index file and current HEAD commit, paths that have differences between working tree and index file, paths in working tree that are not tracked
# use git log = lists commits done in repo in reverse chronological order (recent first), lists each commit with SHA-1 checksum, author, email, date, commit message
# use git branch = pointer to snapshot of changes
# q = exit
git config user.name "Name"
git config user.email "user@example.com"

# create improve-output branch
git branch improve-output
# move from master to improve-output
git checkout improve-output
# open food_count.py with nano
nano food_count.py
# add line on top of loop (with open part) 
print("Favourite foods, from most popular to least popular")
# ctrl -o, enter, ctrl-x to save
# run
./food_count.py
# commit changes to staging area
git add food_count.py
# commit changes
git commit -m "Adding a line in the output describing the utility of food_count.py script"
# output:
[improve-output 09b9ce4] Adding a line in the output describing the utility of food_count.py script
 1 file changed, 1 insertion(+), 1 deletion(-)


# fix the script
# run again
./food_question.py
# output:
Traceback (most recent call last):
  File "./food_question.py", line 10, in <module>
    if item not in counter:
NameError: name 'item' is not defined

# colleague says the file was fine before, so revert back to previous commit
# check git log history to revert back to working commit
git log
# output:
commit 09b9ce441654a361477405d3eea785b3be5f8e8f (HEAD -> improve-output)
Author: Name <user@example.com>
Date:   Thu Oct 12 10:48:08 2023 +0000

    Adding a line in the output describing the utility of food_count.py script

commit 21cf376832fa6eace35c0bf9e4bae4a3400452e9 (master)
Author: Alex Cooper <alex_cooper@gmail.com>
Date:   Wed Jan 8 14:09:39 2020 +0530

    Rename item variable to food_item.

commit b8d00e33237b24ea1480c363edd972cf4b49eedf
Author: Alex Cooper <alex_cooper@gmail.com>
Date:   Wed Jan 8 14:08:35 2020 +0530

    Added file food_question.py that returns how many others in the list like that same food.
# note commit ID for "Rename item variable to food_item"
git revert <commit ID>
# add commit message
# ctrl -o, enter, ctrl-x to save
# run
./food_question.py
# answer prompt with input

# switch to master branch
git checkout master
# merge improve-output into master
git merge improve-output
# output:
Updating 21cf376..70bd534
Fast-forward
 food_count.py    | 2 +-
 food_question.py | 2 +-
 2 files changed, 2 insertions(+), 2 deletions(-)
# run again
./food_question.py
# answer prompt with input
# git status
git status
# output:
On branch master
nothing to commit, working tree clean

# track git commit logs
git log
# output:
commit 70bd534f4d7c9eb2a134a473de578bd2857b66d9 (master)
Author: Name <user@example.com>
Date:   Thu Oct 12 10:53:17 2023 +0000

    Revert "Rename item variable to food_item."
    
    This reverts commit 21cf376832fa6eace35c0bf9e4bae4a3400452e9.

commit 09b9ce441654a361477405d3eea785b3be5f8e8f
Author: Name <user@example.com>
Date:   Thu Oct 12 10:48:08 2023 +0000

    Adding a line in the output describing the utility of food_count.py script

commit 21cf376832fa6eace35c0bf9e4bae4a3400452e9
Author: Alex Cooper <alex_cooper@gmail.com>
Date:   Wed Jan 8 14:09:39 2020 +0530

    Rename item variable to food_item.
# q to exit
