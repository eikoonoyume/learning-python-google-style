# Collabo on GitHub
# forking = creating copy of repo so it belongs to user
# user can push changes to forked copy
# pull request = commit/series of commits that you send to owner of repo, they can incorporate into their tree
# to make a pull request you can make change proposal and write description of change
# allow edits from maintainers as there will be more changes over time
# then click Create pull request

# GitHub Pull Request Workflow
# create a fork of repo by pressing Fork button
# copy URL, use git clone to make local copy on computer
git clone https://github.com/redquinoa/rearrange.git
# output:
Cloning into 'rearrange'...
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 9 (delta 1), reused 9 (delta 1), pack-reused 0
Unpacking objects: 100% (9/9), done.

# rearrange directory now in repo
# look at contents
cd rearrange
ls -l
# output:
total 20
-rw-rw-r-- 1 user user 11357 Jan 7 09:42 LICENSE
-rw-rw-r-- 1 user user   211 Jan 7 09:42 rearrange.py
-rw-rw-r-- 1 user user   762 Jan 7 09:42 rearrange_test.py

# look at commit history
git log
# output:
commit 367a127672c40a163a6f05ad930f2b0b857dc961  (HEAD -> master, origin/master, origin/HEAD)
Author: Blue Kale <bluekale@example.com>
Date:   Mon Jul 29 21:21:53 2019 +0200
    Add tests for the rearrange module
commit c89805e52a1afa143c503f946cc5ead0fdd20255
Author: Blue Kale <bluekale@example.com>
Date:   Mon Jul 29 21:20:57 2019 +0200
    Add the rearrange module
commit f4ddbc7a0ca3ac83a7e9ce7030e774b58e5dda42
Author: Blue Kale <53440916+blue-kale@users.noreply.github.com>
Date:   Mon Jul 29 16:07:42 2019 -0300
    Initial commit

# create a README.md file
# use git checkout -b add -readme to create branch called Add README
git checkout -b add -readme
# output:
Switched to a new branch 'add-readme'

# edit README.md file
# .md file extension = markdown file, lightweight markup language, used to write plain text files with simple rules
# add title and description of file
git atom README.md


Rearrange
=========


This module is used for rearranging names.

# save and commit
git add README.md
git commit -m 'Add a simple README.md file'
# output:
[master 736d754] Add a simple README.md file
 1 file changed, 4 insertions(+)
 create mode 100644 README.md

# push change to forked repo
# use git push -u origin add -readme to create corresponding remote branch 
git push -u origin add -readme
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 400 bytes | 400.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a pull request for 'add-readme' on GitHub by visiting:
remote:      https://github.com/redquinoa/rearrange/pull/new/add-readme
remote: 
To https://github.com/redquinoa/rearrange.git
 * [new branch]      add-readme -> add-readme
Branch 'add-readme' set up to track remote branch 'add-readme' from 'origin'.

# check in GitHub site how it looks when rendered
# click Pull Request
# Write that you're adding a README file that was missing for the project
# Create Pull Request button

# Updating Existing Pull Request
# add more details to file
atom README.md


Rearrange
=========


This module is used for rearranging names. 
Turns "LastName,FirstName" into "Firstname LastName"


# Example


Calling `rearrange_name("Turing, Alan")` will return `"Alan Turing"`

# add changes and commit to repo, write a commit message as well
git commit -a -m 'Add more information to the README'
# output:
Code output: 
[add-readme 01231b0] Add more information to the README
 1 file changed, 5 insertions(+)

git push
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': redquinoa
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 407 bytes | 407.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/redquinoa/rearrange.git
   736d754..01231b0  add-readme -> add-readme

# check pull request in GitHub, there should be two commits
# if you want separate pull request, you need to create new branch
# commit colors: green = new, red = deleted, highlight = only part of line changed


# Squashing Changes
# use git rebase -i to create a single commit that includes both changes and a more detailed description
git rebase -i master
# output:
pick 736d754 Add a simple README file
pick 01231b0 Add more information to the README


(...)
# default first word is "pick" action = takes the commits and rebases them against selected branch
# change first word to select what we want to do with commits
# 2 options for combining commits: squash and fix up (both combine selected commit with previous commit)
# squash = commit messages are added together and editor opens to let us make necessary changes
# fix up = commit message for that commit is discarded

# change pick command to squash
pick 736d754 Add a simple README file
squash 01231b0 Add more information to the README


(...)
# save and exit: ctrl -o, enter, ctrl-x

# edit combined commit message
# This is a combination of 2 commits. 
# This is the 1st commit message:


Add a simple README file


# This is the commit message #2:


Add more information to the README


(...)

# add more info about the change, add that we're including an example use case

# This is a combination of 2 commits. 
# This is the 1st commit message:


Add a simple README file including an example use case


# This is the commit message #2:


Add more information to the README


(...)
# output:
git rebase -i master
[detached HEAD ae779e4] Add a simple README.md file including an example use case
 Date: Tue Jan 7 09:47:17 2020 -0800
 1 file changed, 9 insertions(+)
 create mode 100644 README.md
Successfully rebased and updated refs/heads/add-readme.

# save and exit
# use git show to check output and see latest commit and changes in it
git show
# output:
commit ae779e430288b082a19062ed087c547e1051a981 (HEAD -> add-readme)
Author: My name <me@example.com>
Date:   Tue Jan 7 09:47:17 2020 -0800
    Add a simple README file including an example use case
diff --git a/README.md b/README.md
new file mode 100644
index 0000000..5761a46
--- /dev/null
+++ b/README.md
@@ -0,0 +1,9 @@
+Rearrange
+=========
+
+This module is used for rearranging names.
+Turns "LastName, FirstName" into "FirstName LastName"
+
+# Example
+
+Calling `rearrange_name("Turing, Alan")` will return `"Alan Turing"`

# use git status to check info about current state before pushing change to repo
git status
# output:
On branch add-readme
Your branch and 'origin/add-readme' have diverged,
and have 1 and 2 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)
nothing to commit, working tree clean
# one commit for local branch, 2 commits for origin/add-readme branch

# check graph history of commits (--all for all branches, -4 for last 4 commits)
git log --graph --oneline --all -4
# output:
* ae779e4 (HEAD -> add-readme) Add a simple README.md file including an example use case
| * 01231b0 (origin/add-readme) Add more information to the README
| * 736d754 Add a simple README.md file
|/  
* 367a127 (origin/master, origin/HEAD, master) Add tests for the rearrange module
# first commit * ae779e4 = local add-readme branch
# next two commits = different branch

# git push
git push
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': redquinoa
To https://github.com/redquinoa/rearrange.git
 ! [rejected]        add-readme -> add-readme (non-fast-forward)
error: failed to push some refs to 'https://github.com/redquinoa/rearrange.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

# can't be fast-forwarded
# replace the old commits with new one
# call git push -f to FORCE git to push the current snapshot
git push -f
# output:
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 510 bytes | 510.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/redquinoa/rearrange.git
 + 01231b0...ae779e4 add-readme -> add-readme (forced update)

# chech history again
git log --graph --oneline --all -4
# output:
* ae779e4 (HEAD -> add-readme, origin/add-readme) Add a simple README.md file including an example use case
* 367a127 (origin/master, origin/HEAD, master) Add tests for the rearrange module
* c89805e Add the rearrange module
* f4ddbc7 Initial commit
# just one commit on top of master, they have been combined

# Code Reviews
# reviewer comments: add period at end of sentences, add another hashtag to make title render and smaller font, add a few more examples
# use star * character = lets us create bullet points
atom README.md


Rearrange
=========


This module is used for rearranging names.
Turns "LastName, FirstName" into "FirstName LastName".


## Examples


 * Calling `rearrange_name("Turing, Alan")` will return `"Alan Turing"`
 * Calling `rearrange_name("Hopper, Grace M.")` will return `"Grace M. Hopper"`
 * Calling `rearrange_name("Voltaire")` will return `"Voltaire"`
# save and commit
# this change is to be a part of the previous commit 
# use git commit -a --amend = edit original commit
git commit -a --amend
# output:
[add-readme 55e32ed] Add a simple README.md file including an example use case
 Date: Tue Jan 7 09:47:17 2020 -0800
 1 file changed, 11 insertions(+)
 Create mode 100644 README.md

# check status
git status
# output:
On branch add-readme
Your branch and ‘origin/add-readme’ have diverged,
And have 1 and 1 different commits, respectively
  (use “git pull” to merge the remote branch into yours)
Nothing to commit, working tree clean
# not safe to --amend to commits that have been pushed to repo
# --amend = pretty much the same as creating a new commit, replaces commit and gets completely different commit ID
# to push, force it with -f
git push -f
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': redquinoa
numerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 553 bytes | 553.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/redquinoa/rearrange.git
 + ae779e4...55e32ed master -> master (forced update)
# force pushing is okay for pull request branches, but not good for public repos
# comment is now outdated, resolve conversation on GitHub

# GitHub issues and pull requests have unique number associted using # number format
# possible to automatically close issue directly after merging, 
# include string like "closes:#4" in commit message or as part of pull request description

# update documentation
cd health-checks/
atom README.md
# use inverted quotes to show monospace text, print "Everything ok" if all checks pass, or print error
# health-checks


This repo will be populated with lots of fancy checks. 


Currently the main script is health_checks.py


This script will print "Everything ok" if all checks pass, 
or the corresponding error messages if some checks fail.

# save and commit using git commit -a to edit commit message in editor
# add the string "closes #1) so issue will automatically close once this commit is integrated into main tree
git commit -a
# type
Update README to use the new name of the script


Also add more information about how this works. 
Closes #1
(...)
# output:
[master 8981efe] Update README to use the new name of the script
 1 file changed, 4 insertions(+), 1 deletion(-)

# push to repo
git push
# output:
Username for 'https://github.com': redquinoa
Password for 'https://redquinoa@github.com': 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 564 bytes | 564.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/redquinoa/health-checks.git
   160b5f3..8981efe  master -> master

# click on commit ID to see full commit

# Qwiklabs: Push local commits to GitHub
# fork google/it-cert-automation-practice
# clone repo
git clone https://github.com/<github username>/it-cert-automation-practice.git
# username and password/personal access token
# output:
Cloning into 'it-cert-automation-practice'...
remote: Enumerating objects: 55, done.
remote: Total 55 (delta 0), reused 0 (delta 0), pack-reused 55
Receiving objects: 100% (55/55), 15.11 KiB | 619.00 KiB/s, done.
Resolving deltas: 100% (20/20), done.

# go to it-cert-automation-practice directory
cd ~/it-cert-automation-practice
# use git remote -v to see current remote repo, verifyt hat you have set up remote for upstream repo and origin
git remove -v
# output:
origin	https://github.com/XXXXXXXX/it-cert-automation-practice.git (fetch)
origin	https://github.com/XXXXXXXX/it-cert-automation-practice.git (push)
# downstream = copy/clone/checkout from repo and info flows down to tyou
# upstream = sending changes back into repo
# set upstream for the fork you created
git remote add upstream https://github.com/<github username>/it-cert-automation-practice.git
# verify new upstream repo
git remote -v
# output:
origin	https://github.com/XXXXXXXX/it-cert-automation-practice.git (fetch)
origin	https://github.com/XXXXXXXX/it-cert-automation-practice.git (push)
upstream	https://github.com/XXXXXXXX/it-cert-automation-practice.git (fetch)
upstream	https://github.com/XXXXXXXX/it-cert-automation-practice.git (push)

# configure user and email
git config --global user.name "Name"
git config --global user.email "user@example.com"

# create new branch 
git branch improve-username-behavior
# go to this branch
git checkout improve-username-behavior
# navigate to working directory
cd ~/it-cert-automation-practice/Course3/Lab4
# list files in directory
ls
# output: validations.py
# open validations.py script
cat validations.py
# output:
#!/usr/bin/env python3

import re

def validate_user(username, minlen):
    """Checks if the received username matches the required conditions."""
    if type(username) != str:
        raise TypeError("username must be a string")
    if minlen < 1:
        raise ValueError("minlen must be at least 1")
    
    # Usernames can't be shorter than minlen
    if len(username) < minlen:
        return False
    # Usernames can only use letters, numbers, dots and underscores
    if not re.match('^[a-z0-9._]*$', username):
        return False
    # Usernames can't begin with a number
    if username[0].isnumeric():
        return False
    return True
# script must validate usernames that start with a letter only
# edit this script using nano
nano validations.py
# add these lines
print(validate_user("blue.kale", 3)) # True
print(validate_user(".blue.kale", 3)) # Currently True, should be False
print(validate_user("red_quinoa", 4)) # True
print(validate_user("_red_quinoa", 4)) # Currently True, should be False

# save and exit: ctrl-o, enter, ctrl-x
# run validations.py on python3 interpreter
python3 validations.py
# output:
True
True
True
True
# fix script using nano
nano validations.py
# add
     # Usernames can only use letters, numbers, dots and underscores
    if not re.match(r'^[a-zA-Z][a-z0-9._]*$', username):
        return False
)
# save and exit: ctrl-o, enter, ctrl-x
# run validations.py
python3 validations.py
# output:
True
False
True
False

# check status
git status
# output:
On branch improve-username-behavior
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   validations.py

no changes added to commit (use "git add" and/or "git commit -a")

# add file to staging area
git add validations.py
# check
git status
# output:
On branch improve-username-behavior
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   validations.py
# commit changes
git commit
# editor: type your commit message
# append the line "Closes: #1" at beginning of commit message to close this issue
Closes: #1
Updated validations.py python script.
Fixed the behavior of validate_user function in validations.py.

# save and exit: ctrl -o, enter, ctrl -x
# output:
[improve-username-behavior d947d11] Closes: #1 Updated validations.py python script. Fixed the behavior of validate_user function in validations.py.
 1 file changed, 5 insertions(+), 2 deletions(-)

# push to repo
git push origin improve-username-behavior
# username and password/personal access token
# output:
Username for 'https://github.com': XXXXXXXX
Password for 'https://XXXXXXXX@github.com': 
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 2 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (5/5), 552 bytes | 552.00 KiB/s, done.
Total 5 (delta 2), reused 3 (delta 1), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote: 
remote: Create a pull request for 'improve-username-behavior' on GitHub by visiting:
remote:      https://github.com/XXXXXXXX/it-cert-automation-practice/pull/new/improve-username-behavior
remote: 
To https://github.com/XXXXXXXX/it-cert-automation-practice.git
 * [new branch]      improve-username-behavior -> improve-username-behavior
# create pull request in GitHub

# Git Commands
# Create a new feature branch
git checkout -b feature/user-authentication
# View code changes
git diff
# View commit history
git log
# Create a new tag
git tag v1.0.0
# Compare branches
git diff feature/user-authentication main
# Merge changes from feature branch to main
git checkout main
git merge feature/user-authentication
# Delete feature branch
git branch -d feature/user-authentication

# Undoing Things
# Stash changes
git stash
# Restore changes from stash
git stash pop
# Undo changes in working directory
git checkout -- <file>

# Solve Conflicts
# Attempt to automerge
git merge feature/user-authentication
# Resolve conflicts manually
# Edit files to resolve conflicts
git add <resolved-files>
git commit -m "Resolved conflicts"

# Pull requests and code reviews
# Push changes and open pull request
git push origin feature/user-authentication
# Automated tests run in CI/CD pipeline
# Pull request is reviewed
# Feedback is addressed


