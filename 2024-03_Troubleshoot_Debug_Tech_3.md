# System Crash
-first, look at logs
-check if user can reproduce problem on different computer
-narrow scope to machine-specific problem or other
-check if problem happens reliably
-check if problem is with application or whole system
-good quick fix for the user may be to just have a spare computer available
-check RAM, see if memory chip deteriorated
-use memtest86 tool to check RAM health and look for errors, run on boot instead of normal OS so it can access all available memory and verify if data written is same when read back
-every OS has harddrive checking tools, familiarize with them

# logs for each OS
-Linux: open system log files and VAR log, or user log and dot accession errors file
-Mac: console app
-Windows: Event Viewer
-logs have date and time, try to find error message when the application crashed
-cryptic messages can be searched online where there's either official documentation or posts
-enable sling debut logging to find out more about the error
-enable debugging logging = applications generate a lot more output
-Linux: use strace to see what system calls are being called by programs
-Max: use dtruss
-Windows: use process monitor
-look for changes between time it worked fine and time it started crashing
-check configuration management system for setings, check values stored in Version Control System, look at history of changes
-make/find a reproduction case to help you debug, start with clean slate and slowly put pieces in place until crash triggers
-reproduction case should be as small as possible
-even if not successful, reproduction case info is extremely helpful

# When you can't change the code
-write script that pre-processes data and make sure it's in format that program expects
-Wrapper = function or program that provides a compatibility layer between two functions or programs so they can work well together
-check environment application developers recommend, modify systems to match or run application inside a virtual machine/container
-virtual machine/container = let you run affected application in its own environment without interfering with rest of system
-deploy watchdog
-watchdog = process that checks whether a program is running and if it's not then it will start the program again
-report bugs to application developers
-reports should answer: what were you trying to do? what were the steps you followed? what did you expect to happen? what was the actual outcome?

# Internal Server Error
-500 error = usually means that something on server side of application crashed, no idea what
# check failing webpage
site.example.com/blogs
# connect to web server to figure out the error
ssh webserver
(input password)
# go to Bar log on Linux
# use date command = check current date
# use ls -lt command = sorts files by last modified date, connecting to head command to keep just top 10 lines
date
# output: Thu Jan 9 13:41:09 PST 2020
cd /var/log/
ls -lt | head
# check last lines in syslog using tail
tail syslog
# port 80 = default web serving port
# use netstat command = give bunch of info about network connections depending on flags we pass, accesses bunch of sockets that are restricted to route the administrator user
# call with sudo, lets us run commands as root
# pass flags to netstat
# use -n flag = print numerical addresses instead of resolving host names
# use -l flag = check out sockets that are listening for connection
# use -p flag = print process ID and name to which each socket belongs
# use grep command :80 = checking only port 80
sudo netstat -nlp | grep :80
# nginx = popular web serving application
# check configuration files for this stored in etc director
clear
ls -l /etc/nginx/
# look for configuration related to specific site
ls -l /etc/nginx/sites-enabled/
# open with VI editor
vim /etc/nginx/sites-enabled/site.example.com.conf
# uwsgi_pass = common solution used to connect web server to programs that generate dynamic pages
# exit VI
:q
# look for configuration for uwsgi
ls -l /etc/uwsgi
# check what's in apps-enabled
ls -l /etc/uwsgi/apps-enabled/
# found uwsgi configuration for site, check it
vi /etc/uwsgi/apps-enabled/site.example.com.ini
# exit
:q
# check log file
ls -l site.log
# log file size = 0, strange
# look at python script executed by uwsgi srv/site.example.com prod.py
vi /srv/site.example.com/prod.py
# bottle = pythond module to generate dynamic web pages
# bottom of results = configuration for logs page is currently failing
# colleague's comment mentions how to debug info by uncommenting line that calls bottle.debug
# need write access to file, VI is currently in read only mode
# exit
# open with sudo
sudo vi /srv/site.example.com/prod.py
# bottle.debug() --> bottle.debug()
# save
# reload uwsgi by running sudo service uwsgi reload
sudo service uwsgi reload
# reload website
site.example.com/blogs
# traceback of error appears, issue = permission denied error when trying to open var/log/site.log
# check for other files that start with the site
ls -l site*
# site.log, site.log.1 file = logs are rotating to avoid getting too big
# app currently running with www-data user. If site.log belongs to root user, app can't read or write to this log file, must change owner
sudo chown www-data site.log
# reload page
site.example.com/blogs
# finished short term solution

# long-term remediation = find out what was wrong with ownership, and log rotate configuration


# Accessing Invalid Memory
-when process tried to access a portion of system's memory that wasn't assigned to it
-error like segmentation fault (segfault) or general protection fault will be raised
-pointers = variables that store memory addresses, can be modified as needed
-pointer that is set to value outside of valid memory range is pointing to invalid memory
-attach debugger to faulty program
-executable binary needs to include extra info needed for debugging = debugging symbols
-recompile binary to include symbols or download the symbols from provider of software
-valgrind = Linux tool that can tell us if code is doing any invalid operations no matter if it crashes or not, lets us know if code is accessing variables before initializing them
-Dr. Memory = similar tool for Windows and Linux
-if all else fails, you can always contact developers

# Unhandled Errors and Exceptions
-program may finish unexpectedly with errors like type error, attribute error, division by zero error
-make sure instead of crashing unexpectedly, you get program to inform user of problem and tell them what they need to do
-traceback shows lines of the different functions that were being executed when the problem happened
-use debugging tools like PDB interactive debugger = Python program which lets us do all the typical debugging actions like executing lines one-by-one or looking at how variables change values
-print f debugging = add statements that print data related to code's execution to show contents of variables, return values of functions, metadata like list length or file size
-logging module = python module that lets us set how comprehensive we want code to be, whether we want to include all debug messages or only info warning or error messages
-modify code to catch error, tell user what problem it is, and then finish if needed, it should also include details of host name, port, or username used, etc

# Fixing Someone Else's Code
-common
-spend time getting acquainted with the code
-read comments
-add comments as you read
-read tests associated to code
-write tests of your own to better see what the code was supposed to do
-basically, read the code
-focus on functions or modules, start where the error happened, the function(s) that called it
-start practicing with a program you use and have access code to (ex. web server software and how it parses configuration files, Python module like Python Request and how it processes data it receives, open source project at its list of issues)

# Debugging a Segmentation Fault
./example
# output: segmentation fault
# core file = file that stores all info related to crash
# run ulimit command, -c flag for core files, type unlimited to state that we want core files of any size
ulimit -c unlimited
./example
# output: segmentation fault (core dumped)
# use ls -l to check it out
ls -l core
# output: -rw------ 1 user ussr 380928 Jan 9 14:27 core
# pass file to GDB debugger
# call gdb -c core example = to give it core file and tell it where the executable that crashed is located
gdb -c core example
# output:
...
Program terminated with signal SIGSEGV, Segmentation fault.
#0  __strlen_avx2 () at ../sysdeps/x86_64/multiarch/strlen-avx2.S:65
65	../sys
deps/x86_64/multiarch/strlen-avx2.S: No such file or directory.

# segfault --> program finished, crash happened inside strlen function in file in system libraries
# backtrace command = look at full backtrace of crash
backtrace
# first element = where it crashed, second element = function that called the function that crashed, etc
# strlen was called by copy parameters function, which was called by main function
# use up command = move to the calling function in the backtrace, check out line and copy parameters that caused crash
up
# faulty line is calling strlen function
# call list command to show the lines around it to get more context
list
# there's a for loop using the variable i
# check out value of i using print command
print i
# GDB uses $ followed by number to give separate identifiers to each result it prints
# i's value was 1 at the crash
# i was used to access array called argv
# print contents of first element argv 0, second argv1
print argv[0]
print argv[1]
# 0x numbers = hexadecimal numbers used to show addresses in memory where some data is stored
# GDB says first element is pointer, pointing at ./example string
# second is pointer to zero (null pointer)
# null pointer = 0, 0 is never valid pointer, signals end of data structure in C language
# result = for loop was doing one iteration too many = off-by-one error (very common)
# fix = change less than or equal sign to be strictly less than sign so that iteration stops one element before


# Debug a Python Crash
# check contents of file
cd update_products/
cat new_products.csv

# execute program
./update_products.py new_products.csv
# output: error
# the traceback is like like back-trace but in reverse order, the bottom is where the exception occurred
# Key Error occurred
# use Python debugger pdb3, pass script we want to run and any parameters needed
pdb3 update_products.py new_products.csv
# tell debugger to continue the execution until it finishes or crashes
continue
# print contents of row
print(row)
# BOM (Byte Order Mark) = used in UTF-16 to tell difference between a file stored using Litle-Endian and Big-Endian
# file is in UTF-8, doesn't need BOM
# set encoding parameter to UTF-8sig in open function, Python will then get rid of BOM
atom update_products.py
(find line for open function, add "encoding='utf-8-sig'"
# save and execute
./update_products.py new_products.csv

# advanced debugging features
-breakpoint = let code run until certain line of code is executed 
-watchpoint = let code run until a variable or expression changes

# Debugging with Print
-print statement = helps you figure out what is going on in code
-tell print to send messages to screen as program executes to help find out how far it's getting before crash
-tell print to print value of certain variables as program runs
-add print statement to top of a loop that doesn't seem to execute correctly

# division function for two numbers
def divide(numerator, denominator):
  return numerator / denominator
# dividing by zero = ZeroDivisionError
# add print statement
def divide(numerator, denominator)
  print("dividing {} by {}".format(numerator, denominator))
  return numerator / denominator
# you'll see output printed before error

# LocationValueError
# retrieve web page and print contents
import urllib3
def get_web_page(url):
  http = urllib3.PoolManager()
  response = http.request("GET", url)
  print(response.data.decode("utf-8"))
# if URL is missing, you'll see LocationValueError
# print statement to see what URL is before making the call
import urllib3
def get_web_page(url):
  http = urllib3.PoolManager()

  print("Retrieving URL:", url)
  response = http.request("GET", url)

  print("HTTP response code:", response.status, response.reason)
  print(response.data.decode("utf-8"))
# try again
get_web_page("http://google.com")Retrieving URL: http://google.co HTTP response code : 200 ok
# when in doubt, add print statements, take them out later when done debugging

# Debugging with Assert
-assertion = logical test that developers use as a sanity check when writing code
-in Python, use assert statement
-the condition you include should always be TRUE
-if condition is FALSE, use this info as main indicator that program has a bug
-automatically, it will terminate execution of program and display error message
 a = 3 + 4
 assert a == 7

 # if function is called with empty string instead of filename, it will return FileNotFoundError
 def read_file_and_do_something(filename):
   assert filename != ""
   with open(filename, "r") as fp:
   ...
# assert statement produces nothing

-assertions prevent code from continuing to execute when additional input would cause further errors
-use them to create error messages that are easy to understand and user-friendly
def read_file_and_do_something(filename):
  assert filename != "", "You must specify a filename!"
  with open(filename, "r") as fp:
  ...
# error message is not clear and easy to understand

# Try and Catch Debugging
-"try and catch" = common programming paradigm found in many programming languages (not Python)
-handles runtime errors (or "exceptions") without crashing program
-Python version = "try" and "except" blocks
-use when you anticipate this segment of code might produce error
-follow try block with one or more except blocks to catch specific exceptions
try:
  # code that might raise exception
except SomeExceptionType
  # handle the exception

# ZeroDivisionError
try:
  # code that might raise exception
  result = 10 / 0
except ZeroDivisionError:
  print("Oop! You tried to divide by zero.")

-Benefits: identify problems in code, gain insight using messages, fail gracefully without crashing, log errors in Python's logging module

def calculate_average(numbers):
  return sum(numbers) / len(numbers)
# if empty list is passed, ZeroDivisonError is raised, use "try" and "except"
def calculate_average(numbers):
  try:
    return sum(numbers) / len(numbers)
  except ZeroDivisionError:
    print("The list is empty. Cannot calculate the average.")
    return None

# Get Detailed Info
-exceptions in Python = OBJECTS
-print or log exception object itself to provide detailed info about issue
-use "raise" keyword within except blockis to raise custom/built-in exceptions with more descriptive messages
-define own custom exceptions to provide more specific error feedback
class InvalidInputError(Exception):
  pass
class EmptyInputError(Exception):
  pass
def calculate_average(numbers):
  try:
    return sum(numbers) / len(numbers)
  except TypeError:
    raise InvalidInputError(f"Expected a list or tuple, but got {type(numbers)}")
  except ZeroDivisionError:
    raise EmptyInputError("The list is empty. Cannot calculate the average.")
  finally:
    print("Execution of calculate_average function completed.")
# InvalidInputError and EmptyInputError = custom exceptions
# finally clause = specify actions that must be executed regardless of whether exception was raised or not

# Warning
-"try" and "except" blocks aren't traditional "debugging"
-they provide framework to handle and understand and resolve errors in controlled and informed manner
-use it judiciously
-overusing --> swallowing exceptions (erros are caught but not adequately handled or logged, leading to SILENT FAILURES)

# Python Logging Module
-Python logging module = built-in library designed to provide flexible framework for creating log messages
-provides way to configure and capture log messages at different severity levels
-categorize error messages based on severity: ex. DEBUG, INFO, WARNING, ERROR, CRITICAL 
# import first
import logging
# default levels = WARNING and above
logging.warning('This is a warning message')
logging.error('This is an error message')
# set logging level to DEBUG to capture all messages using this code
logging.basicConfig(level=logging.DEBUG)
logging.debug('This is a debug message')
# configure logging to save messages to file to keep record of log messages
logging.basicConfig(filename='app.log', level=logging.DEBUG)
logging.info('This message will be written to app.log')
# customize format of log message to include info like timestamp, log level, message
logging.basicConfig(format='%(asctime)s - %(levelname)s - %(message)s', level=logging.DEBUG)
logging.error('This is an error with a custom format')

# example:
def user_login(username, password):
  logging.info(f"Attempting to log in user: {username}")
  # ... (some code for authentication)
  if authentication_failed:
    logging.error(f"Login failed for user: {username}")
  else:
    logging.info(f"Successfully logged in user: {usrename}")
# conclusion: this isn't the preferred log file method. Real-world use prefers HANDLERS
# Handlers = advanced ways to manage and route logs

# Using Python DeBugger (pdb)
-Step 1: set breakpoint where you suspect issue might be
-breakpoint = a spot in code that tells pdb to pause your code at the specific line
pdb.set_trace()
# prompt appears: (pdb) [ ]

import pdb

def add_numbers(a, b):
  pdb.set_trace() # this sets breakpoint
  result = a + b
  return result

print(add_numbers(3, 4))
# execution pauses at pdb.set_trace()
# interactive debugger session appears
-Step 2: query variable and see values
-use n (next) and p (print) to show update
-a = show arguments of current function
-b = manually set persistent breakpoint
-s = execute current line ans stop at first possible occasion
-c = resume normal exectution until next breakpoint
-pp = pretty-print value of expression
-q = exit debugger, terminate program
-r = continue execution until current function returns
-tbreak = manually set temporary breakpoint
-! = prefix to execute arbitrary Python command in current environment

-Step 3: inspect variables
-type "p" and variable name to see current value
ex. p sentiment_score

-Step 4: change value of variable directly in debugger
ex. !sentiment_score = 0.9
-use "a" or directly proble value with "p<value name>" to confirm changes

-Step 5: exit debugger with "q"

-use pdb to inspect program state at time of crash
python -m pdb your_script.py

# Postmortems
-postmortems = documents that describe details of incidence to help us learn from our mistakes
-goal: learn from what happened to prevent same issue
-document: what happened and when, impact of issue, who was affected, why it happened, how it was diagnosed, short term remediation and long term remediation (how it was fixed), figure out what we can do to avoid same event in future
-make sure to include: highlights of root cause, impact, what needs to be done to prevent issue, and what went well
-language: simple and concise, short sentences, basic language, only important info (who, what, when, where, why), stick to the facts, list everything necessary to understand overall picture
-avoid: conjecture, blame, glossing over necessary details
-be honest and accurate

# Qwiklabs: fix errors in Python scripts
# run script
cd /
python3 /usr/bin/infrastructure

# output: ImportError
# ImportError = raised when import statement has trouble importing specific module
# module matplotlib wasn't foud, import this module
# install pip3 Python package installer
sudo apt install python3-pip -y
# install matplotlib python library
pip3 install matplotlib

# matplotlib = plotting library for Python and NumPy(numerical mathematics extension), provides object-oriented API for embedding plots into applications using GUI toolkits, it's a visualization library for 2D plots of arrays

# run script
python3 /usr/bin/infrastructure
# output: NoFileError
# debug time, navigate to working directory, see if data.csv file exists
cd ~
ls
# output: data.bak
# change extension
mv data.bak data.csv
# navigate back to root directory, run script
cd /
python3 /usr/bin/infrastructure
# output: MissingColumnError
# column for "company" is missing

# check data.csv for columns
cat ~/data.csv
# output: firstname,surname,,job title
# time to add column name in between the commas, grant permission to data.csv file
sudo chmod 777 ~/data.csv
# use nano editor
nano ~/data.csv
# add column name in between the commas, save and exit
# run script
python3 /usr/bin/infrastructure
# finished



