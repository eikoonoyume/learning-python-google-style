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

