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

