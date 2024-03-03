# Dealing with Slowness
# first diagnose with one of these tools, find bottleneck
# top = tool on Linux, lets us see which currently running processes are using the most CPU time, using the most memory, how many processes are running, current state
# iotop + iftop = programs that help up see which processes are currently using the most disk IO usage or the most network bandwidth

# MacOS = OS ships with a tool called Activity Monitor which lets us see what's using the most CPU, memory, energy, disk, network
# Windows = couple of OS tools called Resource Monitor, Performance Monitor, Process Explorer (file monitoring, analyzing processes, handles, DLLs)
# Linus = autogrouping (groups processes during CPU intensive workloads, ensures fair CPU cycle distribution based on nice level, but not too effective when  it interferes with traditional processes)

# Resources
# data variable currently being used in a function = data in CPU internal memory
# data related to running program but not currently executing function = in RAM

# Repeated Data Reading
# can you read it once stored on disk? read it from disk afterwards? 
# see if you can put same info directly into process memory and avoid loading every time
# consider a cache = stores data in a form that's faster to access than original form
# webproxy = cache form that stores websites, images, videos accessed often by users behind proxy so they don't have to be downloaded every time
# file contents or libraries accessed often are stored in RAM by OS
# RAM is for info needed now, disk for info not needed now
# swapping happens between RAM and disk

# Basic Solutions to Slowness
# too many apps open = close unnecessary ones
# not enough memory = add more RAM
# running program has memory leak so it takes all available memory = restart or change code, fix bugs
# logrotate program reduces the size of log files
# hardware failures = use OS utilities to diagnose problems
# malicious software

# Slow Web Server
# user ab = tool (Apache Benchmark) to figure out how slow web page is
# ab -n 500 = figures out average timing of 500 requests to that web page
# example: ab -n 500 site.example.com

# connect to web server
ssh webserver
clear

# use top
top

# ffmpeg = program used for video transcoding (converting files from one video format to another)
# bunch of ffmpeg processes in COMMAND column running, basically using up all CPU
# change process priorities, default priority = 0
# use nice = start process with different priority
# use renice = change priority of process that's already running
# exit with q
q
# change priorities
# use pidof = command that receives the process name and returns all the process IDs that have that name
# iterate over the pidof outcome with for loop, call renice for each process ID
# we want lowest possible priority (19)
for pid in $(pidof ffmpeg); do renice 19 $pid; done
# run software again, fix again
# use ps = command to get some more info about processes
# use ps ax = shows us all running processes on computer
# connect output to less = command to scroll through
# use / search key to look for ffmpeg processes
ps ax | less
/ffmpeg
# use locate = command to see where videos from ffmpeg are
# exit with q
q
# call locate/static/001.webm
locate/static/001.webm
# output: /srv/deploy_videos/static/001.webm

# change into that directory, look at contents
cd /srv/deploy_videos/
ls -l

# use grep = check if any of these files contains a call to ffmpeg
grep ffmpeg*

# use vim command line editor
vim deploy.sh
# output:

#!/bin/bash
# The videos need to be in the mp4 format in ordre to serve them
# in the website.
#
# This script runs ffmpeg in parallel to convert all of the webm files to mp4.
echo “Starting video conversion”
for video in static/*.webm; do
	mp4_video=”$(echo “$video” | sed ‘s/\.webm$/.mp4/’)”
	daemonize -c $PWD /usr/bin/ffmpeg -nostats -nostdin -i “$video” “$mp4_video”
done

# daemonize tool = runs each program separately as if it were a daemon
# reason for overload in server
# delete daemonize part, save, exit
#!/bin/bash
# The videos need to be in the mp4 format in ordre to serve them
# in the website.
#
# This script runs ffmpeg in parallel to convert all of the webm files to mp4.
echo “Starting video conversion”
for video in static/*.webm; do
	mp4_video=”$(echo “$video” | sed ‘s/\.webm$/.mp4/’)”
	/usr/bin/ffmpeg -nostats -nostdin -i “$video” “$mp4_video”
done

# use killall = command to stop all processes
# use killall -STOP = flag to send a stop signal but doesn't kill processes completely
killall -STOP ffmpeg

# send CONT signal to one process, wait, send to next by iterating through list of processes
for pid in $(pidof ffmpeg); do; done
# create a while loop which will fail once process goes away, add call to sleep to wait one second until next check
for pid in $(pidof ffmpeg); do while kill -CONT $pid; do sleep 1; done; done

# Monitoring Tools
# Windows = Sysinternals - serves as advanced task manager
# Linus = Perf-tools, bcc/BPF, bpftrace
# SAR(System Activity Reporter) = tool to analyze performance trends and identify recurring issues over time

# The Use Method
# essential for optimizing system performance and troubleshooting servers. Helps indentify resource bottlenecks and performance issues by analyizng Utilization, Saturation, Errors
# measures CPUS, memory, storage, network interfaces for busy time, additional workload capacity, errors
# creates resource list and Functional Block Diagram to visualize system's components and interactions
# adaptable to cloud computing environments


# Write Efficient Code
# make computer do less work
# write code that is readable, easy to maintain, easy to understand --> less bugs

# figure out which part of code uses most time
# use profiler = tool that measures the resources code is using, see where memory is allocated and time spent
# specific to programming language
# use gprof = analyze C program
# use c-Profile module to analyze Python program
# restructure code to avoid repeating expensive (takes long time to complete) actions (ex. parsing files, reading data over network, iterating through whole list)


# Data Structures
# Lists = ArrayList(Java), Vector(C++), Array(Ruby), Slice(Go) = add, remove, modify elements in sequence
# Dictionaries = HashMap(Java), Unordered Map(C++), Hash(Ruby), Map(Go) = stores key value pairs

# Expensive Loops
# use with caution
# see if you can call once before looping (ex. read from disk before loop, not every loop)
# read only as much as you really need
# break out of loop once you've found what you were looking for 
# use break in Python

# Keeping Local Results
# create local cache if script gets executed regularly, it stores data in form that's quicker to access
# think about how often to update cache, what happens to out-of-date data
# good for long term, not for things that need to be done right now
# can store modification date to recalculate only if file is newer than the one stored

# Example Slow Script with Expensive Loop
# use time = measure script speed
time ./send_reminders.py "2020-01-13|Example|test1"
# output: Successfully sent reminders to: test1
real 0m0.129s
user 0m0.068s
sys 0m0.013s
# real = amount of actual time it took to execute command, aka wall-clock
# user = time spent doing operations in user space
# sys = time spent doing system level operations
# user and sys value don't necessarily add up to real value

# test with 9 test users
“2020-01-13|Example|test1,test2,test3,test4,test5,test6,test6,test7,test8,test9”
# output: Successfully sent reminders to: test1,test2,test3,test4,test5,test6,test6,test7,test8,test9
real 0m0.296s
user 0m0.222s
sys 0m0.008s

# use pprofile3 = profiler to get data about what's going on
# use -f = flag to call grind file format
# use -o = flag to tell it to store the output in the profile.out file
pprofile3 -f callgrind -o profile.out ./send_reminders.py “2020-01-13|Example|test1,test2,test3,test4,test5,test6,test6,test7,test8,test9”
# output: Successfully sent reminders to: test1,test2,test3,test4,test5,test6,test6,test7,test8,test9

# generated file that can open with any tool that supports the call grand format
# use kcachegrind = graphical interface to look at contents
kcachegrind profile.out

# get_name() function getting the most time, optimize
clear
atom send_reminders.py
# output:
def get_name(contacts, email):
	name = “”
	with open(contacts) as csvfile:
		reader = csv.reader(csvfile)
		for row in reader:
			if row[0] == email:
				name = row[1]
	return name


def send_message(date, title, emails, contacts):
	smtp = smtplib.SMTP(‘localhost’)
	for email in emails.split(‘,’):
		name = get_name(contacts, email)
		message = message_template(date, title, name)
		message[‘From’] = ‘noreply@example.com’
		message[‘To’] = email
		Smtp.send_message[message]
	smtp.quit()
	pass
# once it finds element in list, it should immediately break out of loop but it's iterating through the whole file
# fix to read file once, store values we want in dictionary, use dictionary for lookups
# change get_name() function and turn it into read_names() function to process csv file and store values we want in names dictionary
# store email as key, names as value in names dictionary
# return whole dictionary instead of one name
def read_name(contacts):
	names = {}
	with open(contacts) as csvfile:
		reader = csv.reader(csvfile)
		for row in reader:
			names[row[0]] = row[1]:
	return names

# get_name() function is being called once per email, change to call read_names() function before for loop
# get values from dictionary instead of calling get_name
def send_message(date, title, emails, contacts):
	smtp = smtplib.SMTP(‘localhost’)
	names = read_names(contacts)
	for email in emails.split(‘,’):
		name = names[email]
		message = message_template(date, title, name)
		message[‘From’] = ‘noreply@example.com’
		message[‘To’] = email
		Smtp.send_message[message]
	smtp.quit()
	pass

# Profiling
# Timeit module = benchmark through Python, measure execution time of code segments, helsp you pinpoint potential bottlenecks by conducting mini benchmarks for individual functions
# Flat, Call-graph, Input-Sensitive = profiling tools integral to debugging, generate detailed source code reports to understand how an application behaves and uses resources
# useful in developing computer architecture and compilers, good to know to predict program behavior on new hardware configurations, enabling optimization algorithms, etc


# Parallel Operations
# concurrency = field of computer science dedicated to how we write programs that do operations in parallel
# understand what OS already does for you
# split operations across different processes, calling script many times each with a different input set, let OS handle concurrency
# split list of computers into smaller groups and use OS to call script many times once for each group
# have good balance of different workloads that you run on computer
# process that uses a lot of CPU can run parallel to process that uses a lot of network IO and process that uses a lot of disk IO

# threads = let you run parallel tasks inside a process, allows threads to share some memory with other threads in same process
# not handled by OS
# modify code to create and handles threads
# Threading or AsyncIO modules in Python = specify which parts of code we want to run in separate threads or as separate asynchronous events, and how we want results of each to be combined in the end
# if all threads get executed in same CPU procesor, use more processors and split code into fully separate processes
# if script is mostly waiting for input or output, it's I/O bound
# if you're waiting on CPU and using mostly CPU, it's CPU bound

# Other Technology
# SQLite file = lightweight database system that lets you query the info stored in the file without needing to run a database server
# fully-fledged database server, possibly on separate machine
# caching service like memcached = keeps the most commonly used results in RAM to avoid querying the database unnecessarily
# caching service Varnish = speed up laod of dynamically created pages
# load balancer across different computers to distribute requests
# virtual machines running in cloud

# Example of Using Threads
# use time on program
cd thumbnail_generator/
time ./thumbnail_generator.py
# output:
real 0m1.962s
user 0ml.898s
sys 0m0.065s

# import futures submodule, part of concurrent module
from concurrency import futures
import argparse
import logging
import os
import sys
import PTL
import PTL.Image
from tqdm import tqdm

# to run parallel, create exectuor
# executor = process that's in charge of distributing the work among different workers
# futures module provides a couple, one for threads, another for processes
# use ThreadPoolExectuor

def progress_bar(files):
	return tqdm(files, desc=’Processing’, total=len(files), dynamic_ncols=True)
def main():
	process_options()
	# Create the thumbnails directory
	if not os.path.exists(‘thumbnails’):
		os.mkdir(‘thumbnails’)
	executor = futures.ThreadPoolExecutor()
	for root, -, files in os.walk(‘images’):
		for basename in progress_bar(files):
			if not basename.endswith(‘.jpg’):
				continue
# process_file() does the most work, instead of calling it directly in loop, submit new task to executor with name of function and parameters
def progress_bar(files):
	return tqdm(files, desc=’Processing’, total=len(files), dynamic_ncols=True)
def main():
	process_options()
	# Create the thumbnails directory
	if not os.path.exists(‘thumbnails’):
		os.mkdir(‘thumbnails’)
	executor = futures.ThreadPoolExecutor()
	for root, -, files in os.walk(‘images’):
		for basename in progress_bar(files):
			if not basename.endswith(‘.jpg’):
				continue
			executor.submit(process_file, root, basename)
# because of thread, loop will finish as soon as all tasks are scheduled
# add message saying that we're waiting for all threads to finish
# call shutdown function on executor = waits until all workers in pool are done, then shuts down executor
def progress_bar(files):
	return tqdm(files, desc=’Processing’, total=len(files), dynamic_ncols=True)
def main():
	process_options()
	# Create the thumbnails directory
	if not os.path.exists(‘thumbnails’):
		os.mkdir(‘thumbnails’)
	executor = futures.ThreadPoolExecutor()
	for root, -, files in os.walk(‘images’):
		for basename in progress_bar(files):
			if not basename.endswith(‘.jpg’):
				continue
			executor.submit(process_file, root, basename)
	print(‘Waiting for all threads to finish.’)
	executor.shutdown()
return 0
If __name__ == “__main__”:
	sys.exit(main())

# save and test
time ./thumbnail_generator.py
# output:
real 0m1.252s
user 0m2.637s
sys 0m0.295s

# use processes instead of threads to make it faster
# use ProcessPoolExecutor
atom thumbnail_generator.py


def main():
	process_options()
	# Create the thumbnails directory
	if not os.path.exists(‘thumbnails’):
		os.mkdir(‘thumbnails’)
	executor = futures.ProcessPoolExecutor()
	for root, -, files in os.walk(‘images’):
		for basename in progress_bar(files):
			if not basename.endswith(‘.jpg’):
				continue
			executor.submit(process_file, root, basename)
	print(‘Waiting for all threads to finish.’)
	executor.shutdown()
return 0
If __name__ == “__main__”:
	sys.exit(main())
# save and test
time ./thumbnail_generator.py
# output:
real 0m0.945s
user 0m3.277s
sys 0m0.173s

# Qwiklabs: Performance Tuning in Python Scripts
# install pip3 for single-line commands in Python modules
# type Y for prompt
sudo apt install python3-pip
# install psutil python library using pip3
# psutil = process and system utilities, cross-platform library for retrieving info on running processes and system utilization, useful for system monitoring, profiling, etc
pip3 install psutil
# open python3 interpretor
python3
# import psutil python3 module
# check CPU usage and I/O and network bandwidth
import psutil
psutil.cpu_percent()

# output: 0.6
# use psutil.disk_io_counters() and psutil.net_io_counters() to get byte read, byte write for disk I/O, byte received and byte sent for network I/O
psutil.disk_io_counters()
psutil.net_io_counters()
# exit
exit()

# to copy or sync files locally with rsync: rsync -zvh [source files directory] [destination]
# to copy or sync directory locally: rsync -zavh [source files directory] [destination]
# to copy or sync directories recursively locally: rsync -zrvh [source files directory] [destination]
# -v = verbose output
# -q = suppress message output
# -a = archive fiels and directory while synchronizing
# -r = sync files and directories recursively
# -b = take the back up during sync
# -z = compress file data during transfer
# use subprocess module
import subprocess
src = "<source-path>" # source directory
dest = "<destination-path>" # destination directory
# call method, pass arguments
subprocess.call(["rsync", "-arq", src, dest])
# this syncs data recursively from source path to destination path
# exit
exit()

# navigate to script/directory
ls ~/scripts
# output: dailysync.py multisync.py
# open mutisync.py with nano
# define a run method to perform tasks
# a few tasks
# create pool object of Pool class of specific number of CPUs your system has by passing number of tasks you have
# start each tasks within the pool object by calling the map instance method
# pass run function and list of tasks as argument
nano multisync.py
#!/usr/bin/env python3
from multiprocessing import Pool
import os
import subprocess

def run(task):
  # Do something with task here
  print("Handling {}".format(task))
if __name__ == "__main__":
  tasks = ['task1', 'task2', 'task3']
  # create a pool of specific number of CPUs that is same number as tasks
  p = Pool(len(tasks))
  # Start each task within the pool
  p.map(run, tasks)
  
# Save and exit
# grant executable permission 
sudo chmod +x ~/scripts/multisync.py
# run
./scripts/multisync.py

# output:
Handling task1
Handling task2
Handling task3

# open dailysync.py
nano ~/scripts/dailysync.py
# sync from /data/prod to /data/prod_backup folder
#!/usr/bin/env python3

from multiprocessing import Pool
import multiprocessing
import subprocess
import os

home_path = os.path.expanduser('~')
src = home_path + "/data/prod"
dest = home_path + "/data/prod_backup/"

if __name__ = "__main__":
  pool = Pool(multiprocessing.cpu_count())
  pool.apply(subprocess.call, args=(["rsync", "-arq", src, dest],))
# save and exit
# grant executable permission
sudo chmod +x ~/scripts/dailysync.py
# run
./scripts/dailysync.py


