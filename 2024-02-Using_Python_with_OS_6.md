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
# default = input is provided by keyboard at the text terminal, output and errors are shown on screen
# redirection = process of sending a stream to a different destination, provided by OS
# useful if you want to store output in a file
# > greater than = redirect standard output to file
# Python program, print single line of text
cat stdout_example.py
#!/usr/bin/env python3
print("Don't mind me, just a bit of text here...")
./stdout_examply.py
# output: Don't mind me, just a bit of text here...
# > to redirect
./stdout_example.py > new_file.txt
# run, if new_file.txt doesn't exist, it will be created
# look at contents of new_file.txt
cat new_file.txt
# output: Don't mind me, just a bit of text here...

# >> double greater than to append redirected standard out to file
./stdout_examply.py >> new_file.txt
cat new_file.txt
# output: Don't mind me, just a bit of text here...

# redirect standard input
# < less than to read contents of file
cat streams_err.py
#!/usr/bin/env python3

data = input("This will come from STDIN: ")
print("Now we write it out to STDOUT: " + data)
raise ValueError("Now we generate an error to STDERR")
# redirect contents of new_file to script
./streams_err.py < new_file.txt
# output: This will come from STDIN: Now we write it out to STDOUT: Don't mind # me, just a bit of text here...
# Traceback (most recent call last):
  # File "./streams_err.py", line 5, in <module>
    # raise ValueError("Now we generate an error to STDERR")
# ValueError: Now we generate an error to STDERR
# input was read from a file, so it only appears in STDOUT
# input function will only read until it encounters a \n new line character


# redirect STD_err or capture errors and diagnostic meesages from program
# 2> character combo = redirect err to separate file
./streams_err.py < new_file.txt 2> error_file.txt
# This will come from STDIN: Now we write it out to STDOUT: Don't mind # me, just a bit of text here...
# redirected contents of new_file to streams_err.py, the redirected error message to error file
# look at contents of file
cat error_file.txt
# Traceback (most recent call last):
  # File "./streams_err.py", line 5, in <module>
    # raise ValueError("Now we generate an error to STDERR")
# ValueError: Now we generate an error to STDERR
# 2> = the 2 represents the file descriptor of SCD Err stream
# file director = kind of variable pointing to an IO resource
# 0 and 1 = file descriptors for SCDIN and SCDOUT

# create file using ehco command, redirect output to a file we want to create
echo "These are the contents of the file" > myamazingfile.txt
cat myamazingfile.txt
# output: These are the contents of the file

# Pipes and Pipelines
# piping = another IO stream redirection method
# use pipes to connect multiple scripts, commands, programs together into a data processing pipeline
# connect output of one program to input of another, can pass data between programs
# use | pipe character
# allows to create new commands by combining functionality of one command with another without having to store contents in an intermediate file
ls -l | less
# output: (...A list of files appears...)
# output (list of files) of ls command is connected to input of less command (terminal paging program) (displays pages one at a time)
# useful for looking at contents of directory containing a lot of files
# scroll up or down using page up, page down, or arrow keys
# Q = quit

cat spider.txt | tr ' ' '\n' | sort | uniq -c | sort -nr | head
     # 7 the
     # 3 up
     # 3 spider
     # 3 and
     # 2 rain
     # 2 itsy
     # 2 climbed
     # 2 came
     # 2 bitsy
     # 1 waterspout.
# cat = get contents of spider.txt file
# tr = command that gets name from TRANSLATE, takes characters in first parameter (in this case: space)
# transforms into character in second parameter (in this case: \n)
# this means = putting each word in it's own separate line
# sort = command that sorts alphabetically
# uniq = command that displays each match once
# -c = flag that prefixes each uniq line with a number of time it occurred
# sort -nr = flag that sorts results numerically and in REVERSE order from most to least hits
# head = command which prints first 10 lines to stdl


# Python pipes
# python can read standard input using stdin file object in sys module
# file object is already open for reading
# write a script that reads each line of input, prints a line with first character in uppercase
# use capitalize string method
cat capitalize.py
#!/usr/bin/env python3

import sys
# iterate line by line, use strip() method to remove \n character at end, capitalize() method to make first character of line uppercase
for line in sys.stdin:
  print(line.strip().capitalize())

cat haiku.txt
# output: advance your career,
# output: automating with Python,
# output: it's so fun to learn

# use pipeline to capitalize haiku.txt by combining output of cat command with capitalized script
cat haiku.txt | ./capitalize.py
# output: Advance your career,
# output: Automating with Python,
# output: It's so fun to learn

# use redirect to get contents to STDIN
./capitalize.py < haiku.txt
# output: Advance your career,
# output: Automating with Python,
# output: It's so fun to learn

# Signaling Processes
# signal = token delivered to running processes to indicate a desired action as a method of communication
# ping = command to send ICMP packets to the machine over the network once per second
ping www.example.come
# PING www.example.com(2606:2800:220:1:248:1893:25c8:1946 (2606:2800:220:1:248:1893:25c8:1946)) 56 data bytes
# output continues and continues...
# interrupt with ctrl-c combo
--- www.example.com ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 8013ms
rtt min/avg/max/mdev = 93.587/93.668/93.719/0.149 ms
# interrupt --> first it prints summary of what it did and the results because it received signal to stop
# SIGINT = signal that ctrl-c sends
# ctrl-z = signal that stops, causes program to stop without actually terminating
ping www.example.com
# PING www.example.com(2606:2800:220:1:248:1893:25c8:1946 (2606:2800:220:1:248:1893:25c8:1946)) 56 data bytes
# SIGSTOP = signal that ctrl-z  sends
# process doesn't finish properly
# make it run again by executing fg
# fg = command that makes program run once more and keep going until interrupted with ctrl-c, ctrl-z, etc
fg
# ping www.example.com
# 64 bytes from 2606:2800:220:1:248:1893:25c8:1946 (2606:2800:220:1:248:1893:25c8:1946): icmp_seq=5 ttl=51 time=93.6 ms
# ctrl-c
--- www.example.com ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 8013ms
rtt min/avg/max/mdev = 93.587/93.668/93.719/0.149 ms

# kill = command to terminate program, run on separate terminal, need to know PID
# SIGTERM = signal that kill sends
# ps = command to find PID, lists currently running processes
# ps ax = lists all running programs in current computer
# grep = command to only keep lines that contain name of desires process

# Creating Bash Scripts
# ps = command that lists all current running processes
# free = command that shows you amount of free memory
# uptime = command that tells you how long the computer has been on
# who = command that lists users currently logged into computer
# echo = command to print
# date = command to print current date
# $ sign parentheses = indicates that output should be passed to echo command

#!/bin/bash
echo "Starting at: $(date)"
echo

echo "UPTIME"
uptime
echo

echo "FREE"
free
echo

echo "WHO"
who
echo

echo "Finishing at: $(date)"
# execute bash scripts with .sh file extension
./gather-information.sh
# output:
Starting at: Mi 22. Mai 17:13:06 CEST 2019
UPTIME
 17:13:06 up 8 days,  1:34,  2 users,  load average: 0,00, 0,00, 0,00
FREE
              total        used        free      shared  buff/cache   available
Mem:        4037132      871336      253940       10032     2911856     2865984
Swap:       2097148        4364     2092784
WHO
user     :0           2019-05-14 15:39 (:0)
user     pts/1        2019-05-14 15:40 (192.168.122.1)
Finishing at: Mi 22. Mai 17:13:06 CEST 2019
# starting and finishing times = same because there are so few operations being done
# more operations = more time

# write commands on same line using semicolons ; to separate them
#!/bin/bash

echo "Starting at: $(date)"; echo

echo "UPTIME"; uptime; echo

echo "FREE"; free; echo

echo "WHO"; who; echo

echo "Finishing at: $(date)"
# execute
./gather-information.sh
# output:
Starting at: Mon 13 May 2019 02:52:11 PM CEST
UPTIME
 14:52:11 up 17 days,  2:35,  1 user,  load average: 0.70, 1.01, 1.16
FREE
              total        used        free      shared  buff/cache   available
Mem:       32912600    19966400     1003304      321672    11942896    12281516
Swap:      20250620      612352    19638268
WHO
user    tty7         2019-04-29 12:19 (:0)
Finishing at: Mon 13 May 2019 02:52:11 PM CEST

# Using Variables and Globs
# assign variables, new conditional operations, execute loops, defined functions, etc using bash
# environment variables = variables set in environment in which command is executing
# = sign to set variables
# prefix name of variable with $ sign
# assign value to name of variable we want to define
example=hello
echo $example
# output: hello
# NO SPACES BETWEEN NAME OF VARIABLE AND EQUAL SIGN OR BETWEEN EQUAL SIGN AND VALUE
example = hello
# output:
Command ‘example’ not found, did you mean:
	Command ‘gexample’ from deb pvm-examples (3.4.6-2build1)
Try: sudo apt install <deb name>

# defined variables are local to where they were defined, if you want to see it outside, you have to export using export keyword
# add lines in between each command: first define variable called line, put dashes in it
#!/bin/bash

line="-------------------------------------------------"


echo "Starting at: $(date)"; echo $line


echo "UPTIME"; uptime; echo $line


echo "FREE"; free; echo $line


echo "WHO"; who; echo $line


echo "Finishing at: $(date)"
# execute
./gather-information.sh

# output:
Starting at: Mi 22. Mai 17:30:30 CEST 2019
-------------------------------------------------
UPTIME
 17:30:30 up 8 days,  1:51,  2 users,  load average: 0,00, 0,00, 0,00
-------------------------------------------------
FREE
              total        used        free      shared  buff/cache   available
Mem:        4037132      862132      444720       10032     2730280     2875336
Swap:       2097148        6156     2090992
-------------------------------------------------
WHO
user     :0           2019-05-14 15:39 (:0)
user     pts/1        2019-05-14 15:40 (192.168.122.1)
-------------------------------------------------
Finishing at: Mi 22. Mai 17:30:30 CEST 2019

# using * in command line will match all filenames that follow format we specify
echo *.py
# output:
action_deprecation.py areas.py capitalize.py charfreq.py  check_deprecation.py health_checks.py hello.py mycheck.py seconds.py  stdout_example.py streams.py test.py validations.py
# *.py = shell turns it into list containing all filenames that end with py in current directory
# put star * at end of expression to get list of all files that start with a certain prefix
echo c*
# gets all files that start with c
# output: capitalize.py charfreq.py check_localhost.sh

# ? symbol can be used to match exactly one character instead of any amount
# can repeat as many times as needed
# 5 ? = 5 characters
echo ?????.py
# output: areas.py hello.py

# Conditional Execution in Bash
# Python used if block to evaluate true or false
# bash uses exit status of commands
# check exit status for command using $?
# exit value of zero = success
# verify that /etc/hosts file contains entry for 127.0.0.1
# grep = returns exit status of zero if it finds one match, returns not zero if no match
cat check_localhost.sh
# output:
#!/bin/bash
# start with if keyword, follow with grep command to check for success
# end with ; then
if grep "127\.0\.0\.1" /etc/hosts; then
# body of conditional, indentation is not mandatory but is nice
  echo "Everything ok"
# else block for when command doesn't finish successfully
else
  echo "ERROR! 127.0.0.1 is not in /etc/hosts"
fi
# fi keyword to finish
# run script
./check_localhost.sh
# output: 127.0.0.1 localhost # printed because grep's default is to print matched exrpession
# output: Everything ok

# test = command to help evaluate conditions received and exit with zero if true, with one if false
if test -n "$PATH"; then echo "Your path is not empty"; fi
# output: Your path is not empty
# -n option for test command = checks if string variable is empty or not
# another way to write that
if [ -n "$PATH" ]; then echo "Your path is not empty"; fi
# output: Your path is not empty
# opening square bracket [ command = alias to test command, to successfully call you need closing square bracket ]

# While Loops in Bash Scripts
#!/bin/bash

n=1
while [ $n -le 5 ]; do
	echo "Iteration number $n"
 	((n+=1))
done

./while.sh
# variable n is used to print messages
# [ $n -le 5 ] = counting from 1 to 5
# -le operator = checks if variable n is less than or equal to 5 
# do keyword = starts loop
# done keyword = finishes loop
# (( double parentheses lets us use arithmetic operations, ex. increment the value of variable n
# execute
# output: Iteration number 1
# output: Iteration number 2
# output: Iteration number 3
# output: Iteration number 4
# output: Iteration number 5

# common for loop to retry command number of times until it succeeds
# useful with commands that use network connections, or access locked resources
# can fail for external reasons and succeed after retry or two
cat while.sh
# output:
#!/usr/bin/env python
import random
value=random.randint(0, 3)
print("Returning: " + str(value))
sys.exit(value)
# uses randint() (random integer) to generate a value between 0 and 3
# prints selected value
# exits with it
./random-exit.py
./random-exit.py
./random-exit.py
# output: Returning: 3
# output: Returning: 2
# output: Returning: 0

# more complex script
# getting value of command line argument using $1
# use $1 in Bash to access first command line argument
#!/bin/bash

n=0
command=$1
# while loop until either command succeeds or end variable reaches a value of 5
while ! $command && [ $n -le 5 ]; do
# if command fails, retry up to 5 times
# in body of loop, sleep for a few seconds
	sleep $n
# increment variable and print the number of free try attempts
	((n+=1))
done;
# call our retry script with random exit command as parameter
./retry.sh ./random
# output: Returning: 3
# output: Retry #1
# output: Returning: 3
# output: Retry #2
# output: Returning: 1
# output: Retry #3
# output: Returning: 3
# output: Retry #4
# output: Returning: 0

# For Loops in Bash Scripts
# Python: sequences are data structures like list or tuple or string
# Bash: construct sequences by listing elements with spaces in between
# iterate over 3 different elements that have names of fruits
cat fruits.sh
# output:
#!/bin/bash
for fruit in peach orange pear; do
	echo "I like $fruit!"
done
# for loop uses the same "do" "done" structure that while loop used
# execute
./fruits.sh

# output: I like peach!
# output: I like orange!
# output: I like pear!

# review globs
# * glob and ? glob are used to create lists of files, lists are separated by spaces so we can use them in loops to iterate over list of files that match criteria
# rename files that end in HTM to lowercase html
# first, run it and note the bugs, then code
cd old_website/
/old_website$ ls -l
# output:
total 0
-rw-r--r-- 1 user user 0 May 24 10:19 about.HTM
-rw-r--r-- 1 user user 0 May 24 10:20 contact.HTM
-rw-r--r-- 1 user user 0 May 24 10:20 footer.HTM
-rw-r--r-- 1 user user 0 May 24 10:20 header.HTM
-rw-r--r-- 1 user user 0 May 24 10:19 index.HTM
# five files need to be renamed
# extract the part before the extension
# basename = command  that takes a filename and extension and then returns the name without extension
/old_website$ basename index.HTM. HTM

# use bash shebang line
#!/bin/bash
# iterate with for loop through all files that end with .HTM
for file in *.HTM; do
# call basename 
	name=$(basename "$file" .HTM)
 	mv "$file" "$name.html"
# use dollar sign parenthesis $ ( = to call command and keep output
# surround file variable with double-quotes "" to allow to work even if file has spaces in its name
# call MV command with old and new names = use "" double quotes for both parameters
done
# add echo in front of MV command
#!/bin/bash
for file in *.HTM; do
	name=$(basename "$file" .HTM)
 	echo mv "$file" "$name.html"
done

# execute
/old_website$ chmod +x rename.sh
/old_website$ ./rename.sh
# output:
mv about.HTM about.html
mv contact.HTM contact.html
mv footer.HTM footer.html
mv header.HTM header.html
mv index.HTM index.html
# now remove echo and make it actually rename files
#!/bin/bash
for file in *.HTM; do
	name=$(basename "$file" .HTM)
 	mv "$file" "$name.html"
  done
/old_website$ ./rename.sh
# nothing prints because it succeeded, check list of contents of directory
/old_website$
/old_website$ ls -l
# ls -l = command in Linux and Unix systems used to list contents of a directory in long format
# output:
total 4
-rw-r--r-- 1 user user  0 May 24 10:19 about.html
-rw-r--r-- 1 user user  0 May 24 10:20 contact.html
-rw-r--r-- 1 user user  0 May 24 10:20 footer.html
-rw-r--r-- 1 user user  0 May 24 10:20 header.html
-rw-r--r-- 1 user user  0 May 24 10:19 index.html
-rwxr-xr-x 1 user user 90 May 24 10:40 rename.sh

# Advanced Command Interaction
# system log file contains trove of info about the system
# tail = command to look at last 10 lines from file right now
# output:
May 24 10:17:01 ubuntu.local CRON[257236]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
May 24 10:18:41 ubuntu.local rsyslogd: -- MARK --
May 24 10:25:19 ubuntu.local systemd[1]: Reloading.
# date and time of entry was added to line
# name of the computer as well
# name and PID of process that trigger the event
# actual event that's being logged

# cut = command that only lets us take only bits of each line using a field delimiter
tail / var/log/syslog | cut -d' ' -f5-
# output:
CRON[257236]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
rsyslogd: -- MARK --
systemd[1]: Reloading.

# cut -d = use space as delimiter
# -f5- = tells that we want to print the field number 5 and everything that comes after

# pipeline commands
cut -d' ' -f5- /var/log/syslog | sort | uniq -c | sort -nr | head
# output:
     41 systemd[1]: Starting Network Manager Script Dispatcher Service...
     41 systemd[1]: Started Network Manager Script Dispatcher Service.
     41 systemd[1]: NetworkManager-dispatcher.service: Succeeded.
     41 nm-dispatcher: req:1 'dhcp4-change' [ens3]: start running ordered scripts...
     41 nm-dispatcher: req:1 'dhcp4-change' [ens3]: new request (1 scripts)
     41 dhclient[757]: DHCPREQUEST for 192.168.122.103 on ens3 to 192.168.122.1 port 67 (xid=0x3a5ff7ed)
     41 dhclient[757]: DHCPACK of 192.168.122.103 from 192.168.122.1 (xid=0xedf75f3a)
     41 dbus-daemon[592]: [system] Successfully activated service 'org.freedesktop.nm_dispatcher'
     41 dbus-daemon[592]: [system] Activating via systemd: service name='org.freedesktop.nm_dispatcher' unit='dbus-org.freedesktop.nm-dispatcher.service' requested by ':1.15' (uid=0 pid=599 comm="/usr/sbin/NetworkManager --no-daemon " label="unconfined")
      9 systemd[1]: Started Run anacron jobs.

# example basestop
#!/bin/bash
# process all files in var/log that end in log
# print top 5 lin lines in each file
for logfile in /var/log/*log; do
	echo "Process: $logfile"
 	cut -d' ' -5f- $loglife | sort | uniq -c | sort -nr | head -5
done
# execute
./toploglines.sh
# output:
...
Processing: /var/log/user.log
     23 system-updater[199481]: DEBUG Command exited with status: 0
     19 system-updater[46682]: DEBUG Command exited with status: 0
     16 system-updater[175060]: DEBUG Command exited with status: 0
     11 /usr/bin/lock: called by /bin/bash for . uid 0, euid 0.
     11 network-manager-dhclient-hooks: Dispatching run of '/etc/dhcp/dhclient-exit-hooks.d/hostname' ...
Processing: /var/log/Xorg.0.log
     87 Printing DDC gathered Modelines:
     87 Modeline "1920x1080"x0.0  141.00  1920 1936 1952 2104  1080 1083 1097 1116 -hsync -vsync (67.0 kHz eP)
     87 EDID vendor "AUO", prod id 5949
     78 vendor "AUO", prod id 5949
     78 DDC gathered Modelines:

# Bash vs Python
# pipelines are powerful, gives info quickly, be careful because it can become unreadable
for i in $(cat story.txt); do B=`echo -n "${i:0:1}" | tr "[:lower:]" "[:upper:]"`; echo -n "${B}${i:1} "; done; echo -e "\n"
# output: Once Upon A Time There Was An Egg Of A Programming Language Called Python

# complex --> write a Python script
cat capitalize_words.py
# output:
#!/usr/bin/env python3
import sys
for line in sys.stdin:
	words = line.strip().split()
 	print(" ".join([word.capitalize() for word in words]))
# take each line of stdin, remove white space, split it into separate words
# use a list comprehension to capitalize each word and end up joining them back with spaces
# print output
# execute as part of a pipeline
cat story.txt | ./capitalize_words.py
# output: Once Upon A Time There Was An Egg Of A Programming Language Called Python

# bash = when operating with files and system commands if it's simple enough that script = self explanatory
# bash = fast on Linux, doesn't work on Windows (use PowerShell)
# Python = when hard to understand the script, python = more flexible and robust

# Qwiklabs: Edit files using substrings
# cat = command that creates single or multiple files, view contents of file, concatenate files, redirect output in terminal or other files
# grep = command "global regular expression print", processes text line-by-line and prints any lines that match specific pattern
# cut = command that extracts given number of characters or columns from file
# delimiter = character or set of characters that separate text strings
# -d option for delimiter separated files
# -f option specifies field, a set of fields, or range of fields to be extracted
# > or >> can be used to redirect STDOUT, if target file doesn't exist then it will be created
# command + > = overwrite existing file
# command + >> = do not overwrite existing file content, APPEND to file

# navigate to data directory
cd data
# list contents of directory
ls
# view contents of files
cat list.txt
# output:
001 jane /data/jane_profile_07272018.doc
002 kwood /data/kwood_profile_04022017.doc
003 pchow /data/pchow_profile_05152019.doc
004 janez /data/janez_profile_11042019.doc
005 jane /data/jane_pic_07282018.jpg
006 kwood /data/kwood_pic_04032017.jpg
007 pchow /data/pchow_pic_05162019.jpg
008 jane /data/jane_contact_07292018.csv
009 kwood /data/kwood_contact_04042017.csv
010 pchow /data/pchow_contact_05172019.csv
# three columns: line number, username, full path to file
# view complete /data directory
ls
# catch all "jane" lines
grep 'jane' ../data/list.txt
# returns all jane and janez 
# output:
001 jane /data/jane_profile_07272018.doc
004 janez /data/janez_profile_11042019.doc
005 jane /data/jane_pic_07282018.jpg
008 jane /data/jane_contact_07292018.csv
# list only "jane"
grep ' jane ' ../data/list.txt
# output:
001 jane /data/jane_profile_07272018.doc
005 jane /data/jane_pic_07282018.jpg
008 jane /data/jane_contact_07292018.csv

# cut with grep command, use whitespace character ' ' as delimiter (denoted by -d)
# fetch results by specifying fields using -f
grep " jane " ../data/list.txt | cut -d ' ' -f 1
# output:
001
005
008

grep " jane " ../data/list.txt | cut -d ' ' -f 2
# output:
jane
jane
jane

grep " jane " ../data/list.txt | cut -d ' ' -f 3
# output:
/data/jane_profile_07272018.doc
/data/jane_pic_07282018.jpg
/data/jane_contact_07292018.csv

# return a range of fields together
grep " jane " ../data/list.txt | cut -d ' ' -f 1-3
# return set of fields together
grep " jane " ../data/list.txt | cut -d ' ' -f 1,3

# test for presence of file, use -e flag
# -e flag = takes file name as parameter and returns True if file exists
# check existence of file jane_profile_07272018.doc
if test -e ~/data/jane_profile_07272018.doc; then echo "File exists"; else echo "File doesn't exist"; fi
# output: File exists

# use > redirection operator to create empty file by specifying file name
> test.txt
# list files
ls
# output:
jane_contact_07292018.csv  jane_profile_07272018.doc  janez_profile_11042019.doc  kwood_pic_04032017.jpg  kwood_profile_04022017.doc  list.txt  pchow_pic_05162019.jpg  test.txt
# append any string to test.txt using >> redirection operator
echo "I am appending text to this test file" >> test.txt
# view contents of file
cat test.txt
# output: I am appending text to this test file
# iterate
for i in 1 2 3; do echo $i; done
# output:
1
2
3

# write script named findJane.sh within scripts directory
# catch all jane lines, store in oldFiles.txt
# navigate to /scripts directory, create new file findJane.sh
cd ~/scripts
nano findJane.sh
# add shebang line
#!/bin/bash

# create text file oldFiles.txt, must be empty, save files with username "jane" in it
> oldFiles.txt
# search for lines with "jane", save files names into variable "files"
# check if file names present in files variable are actually present in file system
# use text command 
# iterate over, add a test expression within the loop
# if item within files variable passes test, add/append it to file
files=$(grep " jane " ~/data/list.txt | cut -d ' ' -f 3
for files in $files; do
	if [ -e $HOME$file ]; then
 		echo $HOME$file >> oldFiles.txt;
	fi
done
# ctrl-o, Enter, ctrl-x to save
# make file executable
chmod +x findJane.sh
# run
./findJane.sh

# view contents of new file
cat oldFiles.txt
# output:
/home/student-02-196e48437fa8/data/jane_profile_07272018.doc
/home/student-02-196e48437fa8/data/jane_contact_07292018.csv

# write a Python script called changeJane.py
# take oldFiles.txt as command line argument
# rename files with new username "jdoe"
# create changeJane.py under /scripts directory using nano editor
nano changeJane.py
# add shebang line
#!/usr/bin/env python3
# import module
import sys
import subprocess
# sys module = provides access to some variables used or maintained by interpreter and to functions that interact with interpreter
# subprocess module = allows you to spawn new processes, connet to Input, Output, Error pipes, get return codes
# oldFiles.txt is passed as command line argument, it's stored in variable sys.argv[1]
# open file from first argument to read its contents using open() method
# assign it variable or use a with block
# hint: traverse each line in file using readlines() method, use line.strip() to remove whitespaces or new lines and fetch old name
# use replace() function to replace "jane" with "jdoe"
string.replace(old_substring, new_substring)

# run() function = subprocess, takes arguments used to launch the process (list or string)
# pass a list of command to be executed, followed by arguments to command
# mv command = rename the files in the file system
# source file/directory = parameters
mv source destination
# pass a list consisting of mv command
# follow with variable storing old name and new name using run()
#!/usr/bin/env python3
import sys
import subprocess

with open(sys.argv[1], 'r')as file:
	for line in file.readlines():
 		old_name = line.strip()
		new_name = old_name.replace('jane', 'jdoe')
  		subprocess.run(['mv', old_name, new_name)
file.close()
# note: if started with "as f" then close with f.close

# make executable
chmod +x changeJane.py
# run script, pass through oldFiles.txt as command line argument
./changeJane.py oldFiles.txt
# navigate to /data directory
cd ~/data
# view renamed files
ls
# output:
janez_profile_11042019.doc  jdoe_contact_07292018.csv  jdoe_profile_07272018.doc  kwood_pic_04032017.jpg  kwood_profile_04022017.doc  list.txt  pchow_pic_05162019.jpg  test.txt

# take phone number dataset, organize format
^\D*(\d{3})\D*(\d{3})\D*(\d{4})$
# ^\D* = matches 0 or more non-digit characters at beginning of string
# (\d{3}) = captures exactly 3 digits (for area code)
# \D* = matches 0 or more non-digit characters between area code and exchange
# (\d{3} ) = captures 3 digit exchange
# \D* = matches 0 or more non-digit characters between exchange and line
# (\d{4})$ = captures exactly 4 digits at end of string

# substitute groups (area code, exchange, number) using backreferences
(\1) \2-\3
# write Python script to read dataset and output corrected numbers
import re
with open("data/phones.csv", "r") as phones:
	for phone in phones:
 		new_phone = re.sub(r"^\D*(\d{3})\D*(\d{3})\D*(\d{4})$, r"(\1) \2-\3", phone)
   		print(new_phone)

