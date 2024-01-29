# Reading, Writing Files in Python
# Open File, Read Single Line
file = open("spider.txt")
print(file.readline())
# output: The itsy bitsy spider climbed up the waterspout.
print(file.readline())
# output: Down came the rain
print(file.readline())
# output: And washed the spider out.
file.close()

# Read Entire File, Close File
file = open("spider.txt")
print(file.readline())
# output: Out came the sun
print(file.readline())
# output: And dried up all the rain
print(file.read())
# output: And the itsy bitsy spider climbed up the spout again 
# reads enter file starting from after "And dried up all the rain"
file.close()

# Using "with open" to avoid forgetting file.close()
with open("spider.txt") as file:
print(file.readline())
# output: The itsy bitsy spider climbed up the waterspout.

# Iterate Through Files, Make everything Upper Case
with open("spider.txt") as file:
  for line in file:
      print(line.upper())
# output:
# THE ITSY BITSY SPIDER CLIMBED UP THE WATERSPOUT.

# DOWN CAME THE RAIN

# AND WASHED THE SPIDER OUT.

# OUT CAME THE SUN

# AND DRIED UP ALL THE RAIN

# AND THE ITSY BITSY SPIDER CLIMBED UP THE SPOUT AGAIN.

# Removing Empty Lines
with open("spider.txt") as file:
  for line in file:
      print(line.strip().upper())
# output:
# THE ITSY BITSY SPIDER CLIMBED UP THE WATERSPOUT.
# DOWN CAME THE RAIN
# AND WASHED THE SPIDER OUT.
# OUT CAME THE SUN
# AND DRIED UP ALL THE RAIN
# AND THE ITSY BITSY SPIDER CLIMBED UP THE SPOUT AGAIN.

# Read, Close, Open, Sort Alphabetically, Print
file = open("spider.txt")
lines = file.readlines()
file.close()

file = open("spider.txt")
lines = file.readlines()
lines.sort()
print(lines)
# output: ['Down came the rain\n', 'Out came the sun\n', 'The itsy bitsy spider climbed up the waterspout.\n', 'and dried up all the rain\n', 'and the itsy bitsy spider climbed up the spout again.\n', 'and washed the spider out.\n']

# Writing Files
with open("novel.txt", "w") as file:
  file.write("It was a dark and stormy night")
# "r" = read, but is optional because it's understood
# write mode = can't read so there's an error, old contents will be deleted immediately, ALWAYS CHECK

# OS Module: layer between Python and OS, allowing interaction without knowing if it's Windows, MAC, Linux etc
# Remove function: delete files
import os
os.remove("novel.txt")
# If you remove again, error because file no longer exists

# Rename function: rename file
os.rename("first_draft.txt", "finished_masterpiece.txt")
# first parameter = old name, second parameter = new name

# OS path sub-module for file information: Exists function: check existence
os.path.exists("finished_masterpiece.txt")
# output: True
os.path.exists("userlist.txt")
# output: False

# Getsize function: return file size in bytes
os.path.getsize("spider.txt")
# output: 192

# Getmtime function: check when last modified "get modified time"
os.path.getmtime("spider.txt")
# output: 1578322923.8999994
# Unix timestamp: number of seconds since January 1st, 1970

# Datetime module: makes Unix timestamps readable
import datetime
timestamp = os.path.getmtime("spider.txt")
datetime.datetime.fromtimestamp(timestamp)
# returns date and time
# output: datetime.datetime(2020, 1, 6, 7, 2, 3, 899999)

# Abspath function: exact location of file "absolute path"
os.path.abspath("spider.txt")
# output: ‘/home/user/spider.txt’

# Getsize function and isfile function: get file size, check existence
import os
file = "file.dat"
if os.path.isfile(file):
  print(os.path.isfile(file))
  print(os.path.getsize(file))
else:
  print(os.path.isfile(file))
  print("File not found")
# output
# False
# File not found

# Getcwd method: check which current directory Python program  is executing in
# pwd command = prints working directory
print(os.getcwd())
# output: /home/user

# mkdir function: create a directory
os.mkdir("new_dir")

# chdir function: change directories
os.chdir("new_dir")
  os.getcwd())
# output: '/home/user/new_dir'

# rmdir function: remove directories, if directory is EMPTY
os.mkdir("newer_dir")
os.rmdir("newer_dir")

# listdir function: return list of all files and sub-directories in a directory
os.listdir("website")
# output: [‘images’, ‘index.html’, ‘favicon.ico’]

# isdir function: checking if they are directories, or not
dir = "website"
  for name in os.listdir(dir):
      fullname = os.path.join(dir, name) # join directory to each file name, creates string with valid full name
  if os.path.isdir(fullname):
    print("{} is a directory".format(fullname))
  else:
    print("{} is a file.".format(fullname))
# output:
# website/images is a directory
# website/index.html is a file
# website/favicon.ico is a file

# join function: concatenate directories

# Reading CSV Files
# Parse = analyze contents

cat csv_file.txt
Sabrina Green,802-867-5309,System Administrator
Eli Jones,684-3481127,IT Specialist
Melody Daniels,846-687-7436,Programmer
Charlie Rivera,698-746-3357,Web Developer

import csv
# open file
f = open("csv_file.txt") 
# parse file using CSV module
csv_f = csv.reader(f)
# iterate through contents
for row in csv_f:
  # unpack values of name, phone number, role
  name, phone, role = row
  # print to screen
  print("Name: {}, Phone: {}, Role: {}".format(name, phone, role))
f.close()
# output:
# Name: Sabrina Green, Phone: 802-867-5309, Role: System Administrator
# Name: Eli Jones, Phone: 684-3481127, Role: IT specialist
# Name: Melody Daniels, Phone: 846-687-7436, Role: Programmer
# Name: Charlie Rivera, Phone: 698-746-3357, Role: Web Developer

# Writing (Generating) CSV Files
# store data in a list
hosts = [["workstation.local", "192.168.25.46"],["webserver.cloud", "10.2.5.6"]]
# open with "with open"
with open('hosts.csv', 'w') as hosts_csv:
# call writer function, put file as parameter
writer = csv.writer(hosts_csv)
# write row function: writes one row at a time
# write rows function: writes all together
writer.writerows(hosts)

# find output outside Python, using cat command
# cat hosts.csv
# workstation.local.192.168.25.46
# webserver.cloud.18.2.5.6

# Reading and Writing CSV Files with DICTIONARIES
# include names of columns as first line of file
cat software.csv
# Output name,version,status,users
# MailTree,5.34,production,324
# CalDoor,1.25.1,beta,22
# Chatty Chicken,0.34,alpha,4
# use DictReader to read and turn each data row into a dictionary
with open('software.csv') as software:
reader = csv.DictReader(software)
  # iterate through to access data using keys
  for row in reader:
  print(("{} has {} users").format(row["name"], row["users"]))
# output:
# MailTree has 324 users
# CalDoor has 22 users
# Chatty Chicken has 4 users

# use DictWriter to generate file from contents of dictionaries
# each element is a row, values come from dictionaries
# list contains keys, name, username, department
users = [ {"name": "Sol Mansi", "username": "solm", "department": "IT infrastructure"},
 {"name": "Lio Nelson", "username": "lion", "department": "User Experience Research"},
 {"name": "Charlie Grey", "username": "greyc", "department": "Development"}]
# define list of keys
keys = ["name", "username", "department"]
# open for writing
  with open('by_department.csv', 'w') as by_department:
    # write with the keys
    writer = csv.DictWriter(by_department, fieldnames=keys)
    # writeheader = creates first line based on the keys
    # writerows = turns list of dictionaries into lines
    writer.writeheader()
    writer.writerows(users)

cat by_department.csv
# output:
# name,username,department
# Sol Mansi,solm,IT Infrastructure
# Lio Nelson,lion,User Experience Research
# Charlie Grey,greyc,Development

# Qwiklabs Assessment: Handling Files
# navigate to the data directory using cd data command
cd data
# ls command: to find data
ls
# output: employees.csv
# cat command to view contents of file
cat employees.csv
# cd command to go to scripts directory
cd ~/scripts
# nano command to create a file: create generate_report.py
nano generate_report.py

# hash bang(shebang) #!: begin with this, continue to interpreter, the rest of file is an argument
#!/usr/bin/env python3

# convert employee data to dictionary
