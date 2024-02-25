# Working with GitHub
# after creating a repository called health-checks, and README file, create a local copy of repository
# use git clone <url of repo>
git clone https://github.com/redquinoa/health-checks.git
# output:
Cloning into 'health-checks'...
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': (PERSONAL ACCESS TOKEN CODE_NOT TOKEN NAME)
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.

# directory named health-checks was automatically created
# change directory, look at contents
cd health-checks/
ls -l
# output:
total 4
-rw-rw-r-- 1 user user 62 Jan  6 14:06 README.md


# repo is empty, add more content to README file
atom README.md
# health-checks
Scripts that check the health of my computers

This repo will be populated with lots of fancy checks.

# done changing, stage the change and commit
git commit -a -m "Add one more line to README.md"
# output:
[master 807cb50] Add one more line to README.md
 1 file changed, 2 insertions(+)


# send changes to remote repo using git push, it'll take all snapshots and send them
git push
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': (PERSONAL ACCESS TOKEN CODE)
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 347 bytes | 347.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/redquinoa/health-checks.git
   3d9f86c..807cb50  master -> master

# make SSH key pair and store public key in our profile so that GitHub recognizes our computer to avoid entering password
# or use credential helper which caches credentials for time window so we don't have to enter password
# enable credential helper using git config --global credential.helper cache
git config --global credential.helper cache
# add user and password, they'll be cached for 15 minutes
# use git pull to retrieve new changes from repo
git pull
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': 
Already up to date.

# try one more time to test the credential helper
git pull
# output:
Already up to date.

# Working with Remotes
# use git clone to get local copy of remote repo
# remote repo is set up with default origin name
# use git remote -v to look at configuration
cd health-checks/
git remote -v
# output:
origin  https://github.com/redquinoa/health-checks.git (fetch)
origin  https://github.com/redquinoa/health-checks.git (push)

# one url will be used to fetch data from repo, the other to push data to repo

# use git remote show origin to get more info about remote
git remote show origin
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': 
* remote origin
  Fetch URL: https://github.com/redquinoa/health-checks.git
  Push  URL: https://github.com/redquinoa/health-checks.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)

# use git branch -r to look at remote branches being currently tracked
git branch -r
# output:
  origin/HEAD -> origin/master
  origin/master
# branches are read only currently

# to modify: pull new changes to local branch, merge with our changes, push changes to repo
# use git status to check staus of changes, can be used for remote branches as well
git status
# output:
On branch master
Your branch is up to date with 'origin/master'.
nothing to commit, working tree clean

# branch is up to date with origin/master branch

# Fetching New Changes
# check changes done by others
# use git remote show origin to see output
cd health-checks/
git remote show origin
# output:
* remote origin
  Fetch URL: https://github.com/redquinoa/health-checks.git
  Push  URL: https://github.com/redquinoa/health-checks.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (local out of date)
# we are out of date because of new changes
# use git fetch to sync data, it copies commits doen in remote rpo to remote branches
git fetch
# output:
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 4 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From https://github.com/redquinoa/health-checks
   807cb50..b62dc2e  master     -> origin/master

# fetch content has been download but not mirrored
# use git checkout to see working tree
# use git log to see commit history
# use git log origin/master to look at current commits, latest commit is pointed out by remote origin/branch
git log origin/master
# output:
commit b62dc2eacfa820cd9a762adab9213305d1c8d344 (origin/master, origin/HEAD)
Author: Blue Kale <bluekale@example.com>
Date:   Mon Jan 6 14:32:45 2020 -0800
    Add initial files for the checks
commit 807cb5037ccac5512ba583e782c35f4e114f8599 (HEAD -> master)
Author: My name <me@example.com>
Date:   Mon Jan 6 14:09:41 2020 -800
    Add one more line to README.md
commit 3d9f86c50b8651d41adabdaebd04530f4694efb5
Author: Red Quinoa <55592533+redquinoa@users.noreply.github.com>
Date:   Sat Sep 21 14:04:15 2019 -0700
    Initial commit
# and local master branch is pointing to previous commit we made
# run git status
git status
# output:
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
nothing to commit, working tree clean
# we're not updated on commits
# use git merge origin/master to perform merge into our local master branch
git merge origin/master
# output:
Updating 807cb50..b62dc2e
Fast-forward
 all_checks.py | 18 ++++++++++++++++++
 disk_usage.py | 24 ++++++++++++++++++++++++
 2 files changed, 42 insertions(+)
 create mode 100755 all_checks.py
 create mode 100644 disk_usage.py

# merged done with fast-forward, check and disk_usage.py files were added

# use git log to see log output of branch, new commit should be there
git log
# output:
commit 1e0a1dfccf01183bfca7e30fb25f115889f95022 (HEAD -> master, origin/master, origin/HEAD)
commit b62dc2eacfa820cd9a762adab9213305d1c8d344 (HEAD -> master, origin/master, origin/HEAD)
Author: Blue Kale <bluekale@example.com>
Date:   Mon Jan 6 14:32:45 2020 -0800
    Add initial files for the checks
commit 807cb5037ccac5512ba583e782c35f4e114f8599 (HEAD -> master)
Author: My name <me@example.com>
Date:   Mon Jan 6 14:09:41 2020 -800
    Add one more line to README.md
commit 3d9f86c50b8651d41adabdaebd04530f4694efb5
Author: Red Quinoa <55592533+redquinoa@users.noreply.github.com>
Date:   Sat Sep 21 14:04:15 2019 -0700
# master branch is now up to date with remote origin/master branch


# Updating Local Repo
# use git pull to do both fetching and merging, it fetches remote copy of current branch and automatically tries to merge with current local branch
git pull
# output:
remote: Enumerating objects: 8, done.
remote: Counting objects: 100% (8/8), done.
remote: Compressing objects: 100% (5/5), done.
Unpacking objects: 100% (6/6), done.
remote: Total 6 (delta 1), reused 6 (delta 1), pack-reused 0
From https://github.com/redquinoa/health-checks
   807cb50..b62dc2e  master       -> origin/master
 * [new branch]      experimental -> origin/experimental
Updating 807cb50..b62dc2e
Fast-forward
 all_checks.py | 15 +++++++++++++++
 1 file changed, 15 insertions(+)
# updated contents were fetches from remote repo, including new branch. Fast-foward merge occurrent to local master branch
# all_checks file was updated as well
# use git log -p -1 to look at changes
git -log -p -1
# output:
commit 922d65950b5325109525a24b71d8df8a46412d04 (HEAD -> master, origin/master, origin/HEAD)
Author: Blue Kale <bluekale@example.com>
Date:   Mon Jan 6 14:42:44 2020 -0800
    Add disk full check to all_checks.py
diff --git a/all_checks.py b/all_checks.py
index fdc4476..e46cdae 100755
--- a/all_checks.py
+++ b/all_checks.py
@@ -1,16 +1,31 @@
 #!/usr/bin/env python3
 
 import os
+import shutil
 import sys
(...)
def(check_reboot): 
	""" Returns True if the computer has a pending reboot."""
	Return os.path.exists(“/run/reboot-required”)
+def check_disk_full(disk, mmin_absolute, min_percent):
+	"""Returns True if there isn’t enough disk space, False otherwise."""
+	du = shutil.disk_usage(disk)
+	# Calculate the percentage of free space
+	percent_free = 100 * du.free / du.total
+	# Calculate how many free gigabytes
# check_disk_full function was added, includes disk_usage.py we saw earlier
# exit with q

# new remote branch called experimental
# use git remote show origin to check output and see new branch info
git remote show origin
# output:
* remote origin
  Fetch URL: https://github.com/redquinoa/health-checks.git
  Push  URL: https://github.com/redquinoa/health-checks.git
  HEAD branch: master
  Remote branches:
    experimental tracked
    master       tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
# create local branch for experimental branch
git checkout experimental
# output:
Branch 'experimental' set up to track remote branch 'experimental' from 'origin'.
Switched to a new branch 'experimental'
# remote branch contents were automatically copied into local branch, working tree is updated
# use git remote update to get contents of remote branches without automatically merging any contents into local branches

# Pull-Merge-Push Workflow
# make a change to all_checks.py script
atom all_checks.py
#!/usr/bin/env python3
(...)
def check_disk_full(disk, min_gb, min_percent):
    """Returns True if there isn't enough disk space, False otherwise."""
    du = shutil.disk_usage(disk)
    # Calculate the percentage of free space
    percent_free = 100 * du.free / du.total
    # Calculate how many free gigabytes
    gigabytes_free = du.free / 2**30
    if percent_free < min_percent or gigabytes_free < min_gb:
        return True
    return False


def main(): 
    if check_reboot():
        print("Pending Reboot.")
        sys_exit(1)
    if check_disk_full(disk="/", min_gb=2, min_percent=10):
        print("Disk full.")
        sys.exit(1)
    
    print("Everything ok")
    sys.exit(0)


main()
# min_absolute --> min_gb
# stage and commit
git add -p
# output:
diff --git a/all_checks.py b/all_checks.py
index e46cdae..a40047c 100755
--- a/all_checks.py
+++ b/all_checks.py
@@ -8,14 +8,14 @@ def check_reboot():
     """Returns True if the computer has a pending reboot."""
     return os.path.exists("/run/reboot-required")
 
-def check_disk_full(disk, min_absolute, min_percent):
+def check_disk_full(disk, min_gb, min_percent):
     """Returns True if there isn't enough disk space, False otherwise."""
     du = shutil.disk_usage(disk)
     # Calculate the percentage of free space
     percent_free = 100 * du.free / du.total
     # Calculate how many free gigabytes
     gigabytes_free = du.free / 2**30
-    if percent_free < min_percent or gigabytes_free < min_absolute:
+    if percent_free < min_percent or gigabytes_free < min_gb:
         return True
     return False
 
Stage this hunk [y,n,q,a,d,j,J,g,/,s,e,?]? y
@@ -27,7 +27,7 @@ def main():
     if check_reboot():
         print("Pending Reboot.")
         sys.exit(1)
-    if check_disk_full("/", 2, 10):
+    if check_disk_full(disk="/", min_gb=2, min_percent=10):
         print("Disk full.")
         sys.exit(1)
 
Stage this hunk [y,n,q,a,d,K,g,/,e,?]? y

git commit -m 'Rename min_absolute to min_gb, use parameter names'
# output:
[master 03923d0] Rename min_absolute to min_gb, use parameter names
 1 file changed, 3 insertions(+), 3 deletions(-)


# push into remote repo
git push
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': 
To https://github.com/redquinoa/health-checks.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/redquinoa/health-checks.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

# can't fast-forward when remote has changes we don't have in local branch
# sync before pushing
git pull
# output:
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/red-quinoa/health-checks
   92d659..a2dc118  master     -> origin/master
Auto-merging all_checks.py
CONFLICT (content): Merge conflict in all_checks.py
Automatic merge failed; fix conflicts and then commit the result.

# conflict found. Use git log --graph --oneline --all to look at tree of commits
git log --graph --oneline --all
# output:
* 03d23d0 Rename min_absolute to min_gb, use parameter names
| * 42dc118 (origin/master, origin/HEAD) reorder conditional to match parameter order
|/  
| * 4d99c56 (origin/experimental, experimental) Empty check_load function
|/  
* 922d659 Add disk full check to all_checks.py
* b62dc2e Add initial files for the checks
* 807cb50 Add one more line to README.md
* 3d9f86c Initial commit

# current commit and commit in origin/master branch have a common ancestor, must do three-way merge
# use git log -p origin/master to look at actual changes in that commit
git log -p origin/master
# output:
commit a2dc1181e5cccf36fec30d6eeefbe569a13883de (origin/master, origin/HEAD)
Author: Blue Kale <bluekale@example.com>
Date:   Mon Jan 6 14:52:23 2020 -0800
    Reorder conditional to match parameter order
diff --git a/all_checks.py b/all_checks.py
index e46cdae..6dda356 100755
--- a/all_checks.py
+++ b/all_checks.py
@@ -15,7 +15,7 @@ def check_disk_full(disk, min_absolute, min_percent):
     percent_free = 100 * du.free / du.total
     # Calculate how many free gigabytes
     gigabytes_free = du.free / 2**30
-    if percent_free < min_percent or gigabytes_free < min_absolute:
+    if gigabytes_free < min_absolute or percent_free < min_percent:
         return True
     return False
commit 922d65950b5325109525a24b71d8df8a46412d04 (HEAD -> master, origin/master, origin/HEAD)
Author: Blue Kale <bluekale@example.com>
Date:   Mon Jan 6 14:42:44 2020 -0800
    Add disk full check to all_checks.py
diff –git a/all_checks.py b/all_checks.py

# colleague changed something in same line we changed something, fix by editing file to remove conflict
# exit with q first
atom all_checks.py
#!/usr/bin/env python3
(...)
def check_disk_full(disk, min_gb, min_percent):
    """Returns True if there isn't enough disk space, False otherwise."""
    du = shutil.disk_usage(disk)
    # Calculate the percentage of free space
    percent_free = 100 * du.free / du.total
    # Calculate how many free gigabytes
    gigabytes_free = du.free / 2**30
<<<<<<< HEAD
    if percent_free < min_percent or gigabytes_free < min_gb:
=======
    if gigabytes_free < min_absolute or percent_free < min_percent:
>>>>>>> a2dc1181e5cccf36fec30d6eeefbe569a13883de
        return True
    return False
(...)

# problem was in the conditional
# option: keep new order colleague made, but use min_gb
(...)
def check_disk_full(disk, min_gb, min_percent):
    """Returns True if there isn't enough disk space, False otherwise."""
    du = shutil.disk_usage(disk)
    # Calculate the percentage of free space
    percent_free = 100 * du.free / du.total
    # Calculate how many free gigabytes
    gigabytes_free = du.free / 2**30
    if gigabytes_free < min_gb or percent_free < min_percent:
        return True
    return False


(...)
# save with ctrl -o, etner ctrl -x

# add all_checks.py file
# git commit to finish merge
git add all_checks.py
git commit
# output:
Merge branch 'master' of https://github.com/redquinoa/health-checks
Fixed check_disk_usage conditional to use the new order and new variable name.
# Conflicts:
#       all_checks.py
#
(...)

# push to remote again finally
git push
# output:
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 2 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 877 bytes | 877.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To https://github.com/redquinoa/health-checks.git
   a2dc118..58351ff  master -> master

# success. use git log --graph --oneline to look at commit history
git log --graph --oneline
# output:
* 58351ff (Head -> master, origin/master, origin/HEAD) Merge branch ‘master’ of https://github.com/redquinoa/health-checks.git
|\
| * 42dc118 (origin/master, origin/HEAD) reorder conditional to match parameter order
* | 03d23d0 Rename min_absolute to min_gb, use parameter names
|/  
| * 4d99c56 (origin/experimental, experimental) Empty check_load function
|/  
* 922d659 Add disk full check to all_checks.py
* b62dc2e Add initial files for the checks
* 807cb50 Add one more line to README.md
* 3d9f86c Initial commit
# latest commit is the merge, then two commits that caused merge conflict

# Pushing Remote Branches
# create branch first, check it out, OR do it simultaneously with git checkout -b <new branch name>
git checkout -b refactor
# output:
Switched to a new branch 'refactor'
# open file
atom all_checks.py
(...)
def main():
    if check_reboot():
        print("Pending Reboot.")
        sys.exit(1)
    if check_disk_full(disk="/", min_gb=2, min_percent=10):
        print("Disk full.")
        sys.exit(1)
(...)
# pattern of repeating code in script
# create a function that checks if disk is full without parameters
# change error message
(...)
def check_root_full():
    """Returns True if the root partition is full, False otherwise."""
    return check_disk_full(disk="/", min_gb=2, min_percent=10)


def main():
    if check_reboot():
        print("Pending Reboot.")
        sys.exit(1)
    if check_root_full():
        print("Root partition full.")
        sys.exit(1)
(...)

# save, test
./all_checks.py
# output:
Everything ok.

# commit
git commit -a -m 'Create wrapper function for check_disk_full'
# output:
[refactor e914aee] Create wrapper function for check_disk_full
 1 file changed, 8 insertions(+), 2 deletions(-)

# to avoid code repetition, create list containing names of functions that we want to call, message to print if function succeeds
# add for loop to iterate over list of checks and messages
# call check, if true then print message, exit with code of 1
# delete old code

(...)
def check_root_full():
    """Returns True if the root partition is full, False otherwise."""
    return check_disk_full(disk="/", min_gb=2, min_percent=10)


def main():
    checks = [
            (check_reboot, "Pending Reboot."),
            (check_root_full, "Root partition full"),
            ]


    for check, msg in checks:
        if check():
            print(msg)
            sys.exit(1)


    print("Everything ok.")
    sys.exit(0)
(...)

# save, test
./all_checks.py
# output:
Everything ok.

# commit
git commit -a -m 'Iterate over a list of checks and messages to avoid code duplication'
# output:
[refactor 75bdd43] Iterate over a list of checks and messages to avoid code duplication
 1 file changed, 8 insertions(+), 6 deletions(-)

# add Boolean variable "Everything ok" before iteration to let script show more than one message if more than one check is failing
# when false, exit with error code only after all checks are done
(...)    
def main():
    checks = [
            (check_reboot, "Pending Reboot."),
            (check_root_full, "Root partition full"),
            ]
everything_ok = True
    for check, msg in checks:
        if check():
            print(msg)
            everything_ok = False


    if not everything_ok:
        sys.exit(1)


    print("Everything ok.")
   sys.exit(0)
(...)

# save, test
./all_checks.py
# output:
Everything ok.

# commit
git commit -a -m 'Allow printing more than one error message'
# output:
[refactor cbee3f7] Allow printing more than one error message
 1 file changed, 7 insertions(+), 1 deletion(-)

# to push branch to remote repo, add a few more parameters to git push
# use git push -u to create branch upstream (push branch to remote repo)
git push -u origin refactor
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': 
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 4 threads
Compressing objects: 100% (9/9), done.
Writing objects: 100% (9/9), 1.34 KiB | 1.34MiB/s, done.
Total 9 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 1 local object.
remote: 
remote: Create a pull request for 'refactor' on GitHub by visiting:
remote:      https://github.com/redquinoa/health-checks/pull/new/refactor
remote: 
To https://github.com/redquinoa/health-checks.git
 * [new branch]      refactor -> refactor
Branch 'refactor' set up to track remote branch 'refactor' from 'origin'.

# Rebashing Changes
# options to merge back into master branch
# use git merge command
# or use git rebase command 
# rebashing = changing the base commit that is used for branch
# if only one branch has changes when merging, Git uses fast-forward 
# if both branches have changes, Git uses three-way merge
# three-way merge split history is hard to debug 
# change base where the commits split from branch history to put the new commits on top of new base to allow fast-forward merge and keep history linear instead of split
# use git rebase command <new branch base>
# let's rebase refactor branch onto master branch
# check out master branch first, pull latest changes in remote repo
git checkout master
# output:
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

git pull
# output:
Remote: Enumerating objects: 5, done. 
Remote: Counting objects: 100% (5/5), done. 
Remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 10% (3/3), done. 
From https://github.com/redquinoa/healthchecks
    58351ff..0789f64  master    -> origin/master
Updating 58351ff..0789f64
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)

# colleague had made changes, fast-forward impossible
# use git log --graph --oneline --all to see current graph of all branches
git log --graph --oneline --all
# output:
* 0789f64 (HEAD -> master, origin/master, origin/HEAD) Add reference to all_checks.py to README
| * cbee3f7 (origin/refactor, refactor) Allow printing more than one error message
| * 75bdd43 Iterate over a list of checks and messages to avoid code duplication
| * e914aee Create wrapper function for check_disk_full
|/  
*   58351ff Merge branch 'master' of https://github.com/redquinoa/health-checks
|\  
| * a2dc118 Reorder conditional to match parameter order
* | 03d23d0 Rename min_absolute to min_gb, use parameter names
|/  
| * 4d99c56 (origin/experimental, experimental) Empty check_load function
|/  
* 922d659 Add disk full check to all_checks.py
* b62dc2e Add initial files for the checks
* 807cb50 Add one more line to README.md
* 3d9f86c Initial commit

# there are 3 commits before common ancestor, current commit at head of master branch
# merging now would be three-way, rebase now 
git checkout refactor
# output:
Switched to branch 'refactor'
Your branch is up to date with 'origin/refactor'.

git rebase master
# output:
First, rewinding head to replay your work on top of it...
Applying: Create wrapper function for check_disk_full
Applying: Iterate over a list of checks and messages to avoid code duplication
Applying: Allow printing more than one error message

# rewound head and replayed our work on top of it

# look at git log --graph --oneline to look at output
git log --graph --oneline
# output:
* f5813b1 (HEAD -> refactor) Allow printing more than one error message
* 18257a0 Iterate over a list of checks and messages to avoid code duplication
* e914aee Create wrapper function for check_disk_full
* 0789f64 (origin/master, origin/HEAD, master) Add reference to all_checks.py to README
*   58351ff Merge branch 'master' of https://github.com/redquinoa/health-checks
|\  
| * a2dc118 Reorder conditional to match parameter order
* | 03d23d0 Rename min_absolute to min_gb, use parameter names
|/  
* 922d659 Add disk full check to all_checks.py
* b62dc2e Add initial files for the checks
* 807cb50 Add one more line to README.md
* 3d9f86c Initial commit

# merge commits back to repo, fast-forward, use check out master branch and merge refactor branch
git checkout master
# output:
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

git merge refactor
# output:
Updating 0789f64..f5813b1
Fast-forward
 all_checks.py | 24 +++++++++++++++++-----
 1 file changed, 19 insertions(+), 5 deletions(-)


# remove remote branch since we're done
# use git push --delete origin refactor
# use git branch -d refactor to remove local branch
git push --delete origin refactor
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': 
To https://github.com/redquinoa/health-checks.git
 - [deleted]         refactor


git branch -d refactor
Code output: 
Deleted branch refactor (was f5813b1).

# push changes back into remote repo
git push
# output:
Enumerating objects: 11, done.
Counting objects: 100% (11/11), done.
Delta compression using up to 4 threads
Compressing objects: 100% (9/9), done.
Writing objects: 100% (9/9), 1.37 KiB | 1.37 MiB/s, done.
Total 9 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 1 local object.
To https://github.com/red-quinoa/health-checks.git
   0789f64..f5813b1  master -> master


# Another Rebase example
# use socket module to check if we can resolve google.com url
# add new funciton called check no network, true if it fails to resolve url, false it if succeeds
# socket.gethostbyname function raises exception on failure, use try except block to wrap call to function, return false if call succeeds, true if it fails


atom all_checks.py


(...)
import socket
(...)
def check_root_full():
    """Returns True if the root partition is full, False otherwise."""
    return check_disk_full(disk="/", min_gb=2, min_percent=10)


def check_no_network():
    """Returns True if it fails to resolve Google's URL, False otherwise."""    
    try:
        socket.gethostbyname("www.google.com")
        return False
    except:
        return True
def main():
    checks = [
            (check_reboot, "Pending Reboot."),
            (check_root_full, "Root partition full"),
            (check_no_network, "No working network."),
            ]
(...)

# add name of function and message "No working network"

# save and commit change
git commit -a -m 'Add a simple network connectivity check'
# output:
[master aa8da6f] Add a simple network connectivity check
 1 file changed, 10 insertions(+), 1 deletion(-)

# use git fetch to put latest changes into origin/master branch without applying to local master
git fetch
# output:
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 5 (delta 1), reused 5 (delta 1), pack-reused 0
Unpacking objects: 100% (5/5), done.
From https://github.com/redquinoa/health-checks
  f5813b1...d8c8fcd master     -> origin/master
# use git rebase against origin/master to rebase changes made by colleague
git rebase origin/master
# output:
First, rewinding head to replay your work on top of it...
Applying: Add a simple network connectivity check
Using index info to reconstruct a base tree...
A       all_checks.py
Falling back to patching base and 3-way merge...
Auto-merging health_checks.py
CONFLICT (content): Merge conflict in health_checks.py
error: Failed to merge in the changes.
Patch failed at 0001 Add a simple network connectivity check
hint: Use 'git am --show-current-patch' to see the failed patch
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".

# conflict in merging, output gives solutions to solve this
# fix conflict
atom health_checks.py


(...)
<<<<<<< HEAD:health_checks.py
def check_cpu_constrained():
    """Returns True if the cpu is having too much usage, False otherwise."""
    return psutil.cpu_percent(1) > 75


def main():
    checks = [
            (check_reboot, "Pending Reboot."),
            (check_root_full, "Root partition full"),
            (check_cpu_constrained, "CPU load too high."),
            ]
    everything_ok = True
=======
def check_no_network():
    """Returns True if it fails to resolve Google's URL, False otherwise."""
    try:
        socket.gethostbyname("google.com")
        return False
    except:
        return True
>>>>>>> Add a simple network connectivity check:all_checks.py


def main():
    checks = [
            (check_reboot, "Pending Reboot."),
            (check_root_full, "Root partition full"),
<<<<<<< HEAD:health_checks.py
            (check_cpu_constrained, "CPU load too high."),
=======
            (check_no_network, "No working network."),
>>>>>>> Add a simple network connectivity check:all_checks.py
            ]
(...)
# keep both new functions and end result, clean up conflict markers >> 
(...)
def check_cpu_constrained():
    """Returns True if the cpu is having too much usage, False otherwise."""
    return psutil.cpu_percent(1) > 75


def main():
    checks = [
            (check_reboot, "Pending Reboot."),
            (check_root_full, "Root partition full"),
            (check_cpu_constrained, "CPU load too high."),
            ]
    everything_ok = True


def check_no_network():
    """Returns True if it fails to resolve Google's URL, False otherwise."""
    try:
        socket.gethostbyname("google.com")
        return False
    except:
        return True


def main():
    checks = [
            (check_reboot, "Pending Reboot."),
            (check_root_full, "Root partition full"),
            (check_cpu_constrained, "CPU load too high."),


            (check_no_network, "No working network."),
            ]
(...)

# save and test
./health_checks.py 
# output:
Traceback (most recent call last: 
  File ".health_checks.py", line 61, in <module>
    main()
  File ".health_checks.py", line 49, in main
    If check():
  File ".health_checks.py", line 30, in check_cpu_constrained
    Return psutil.cpu_percent(1) >75
NameError: name ‘psutil’ is not defined

# forgot to import psutil, fix

atom health_checks.py


import os
import shutil
import sys
import socket
import psutil


(...)

# save and test

./health_checks.py 
# output:
Everything ok.

# add changes, continue to rebase using git rebase --continue

git add health_checks.py 
git rebase --continue
# output:
Applying: Add a simple network connectivity check

# check output with log
git log --graph --oneline
# output:
* 160b5f3 (HEAD -> master) Add a simple network connectivity check
* b52b7a2 (origin/master, origin/HEAD) Add cpu_constrained check
* 9fc7275 Rename all_checks.py to health_checks.py
* f5813b1 Allow printing more than one error message
* 18257a0 Iterate over a list of checks and messages to avoid code duplication
* 5d2e3eb Create wrapper function for check_disk_full
* 0789f64 Add reference to all_checks.py to README 
*   58351ff Merge branch 'master' of https://github.com/redquinoa/health-checks
|\  
| * a2dc118 Reorder conditional to match parameter order
* | 03d23d0 Rename min_absolute to min_gb, use parameter names
|/  
* 922d659 Add disk full check to all_checks.py
* b62dc2e Add initial files for the checks
* 807cb50 Add one more line to README.md
* 3d9f86c Initial commit

# push to remote repo
git push
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 501 bytes | 501.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/redquinoa/health-checks.git
   b52b7a2..160b5f3  master -> master

# Best Practices for Collaboration
# always synchronize branches before starting any work
# try and avoid having very large changes that modify a lot of different things, make changes small, split different commits, push changes often, pull before doing any work, reduce chances of conflict
# use separate feature branch to make big changes
# regularly merge changes made on master branch back onto feature branch
# have latest version of project in master branch, and stable version of project on seperate branch
# should not rebase changes that have been pushed to remote repos
# have good commit messages

# Qwiklab: Introduction to GitHub
# use HTTPS url to clone git repo
# create remote repo
git clone <https://github.com/<username>/<git-repo-name>.git>
# enter username for GitHub, enter Personal Access Token for password
# ls to list files
# cd to move into repo and see project files
cd <directory name = repo name>
# set git username for future commits only
git config --global user.name "Name"
# set email
git config --global user.email "user@example.com"

# edit README file with nano editor
nano README.md
# add text
I am editing the README file. Adding some more details about the project description.
# save: ctrl -o, enter, ctrl -x
# check status
git status
# output:
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

# add file to staging area
git add README.md
# check status
git status
# output:
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md
# commit
git commit
# add commit message: I am editing the README file.
# save and exit

# push to remote repo on main branch
git push origin main
# enter user and password PAT

# create file example.py
nano example.py
# add Python script
def git_opeation():
 print("I am adding example.py file to the remote repository.")
git_opeation()

# save and exit
# add
git add example.py
# commit
git commit
# add message

# in GitHub, add file --> create new file, commit

# push changes on local repo back in the terminal
git push origin main
# output: error

# pull from remote repo to local repo
git pull origin main
# commit message
# save and exit
# use git pull
git pull
# use push
git push origin main
# now local repo is up to date with remote repo




