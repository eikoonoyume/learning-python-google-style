# Reading Files
# Open File
file = open("spider.txt")

# Read Single Line of File, returning as string
file = open("spider.txt")
print(file.readline())
# output:
# The itsy bitsy spider climbed up the waterspout.
print(file.readline())
# output:
# Down came the rain
print(file.readline())
# output:
# And washed the spider out.
file.close()

# Close File *IMPORTANT*
file.close()

# Using WITH to automatically close file
file = open("spider.txt")
print(file.readline())
# output:
# Out came the sun
print(file.readline())
# output:
# And dried up all the rain
print(file.readline())
# output:
# And the itsy bitsy spider climbed up the spout again
file.close()
with open("spider.txt") as file:
print(file.readline())
# output:
# The itsy bitsy spider climbed up the waterspout.

# Iterating Through Files
with open("spider.txt") as file:
  for line in file: # iterate through
    print(line.upper()) # make each line upper case
# output:
# THE ITSY BITSY SPIDER CLIMBED UP THE WATERSPOUT.

# DOWN CAME THE RAIN

# AND WASHED THE SPIDER OUT.

# OUT CAME THE SUN

# AND DRIED UP ALL THE RAIN

# AND THE ITSY BITSY SPIDER CLIMBED UP THE SPOUT AGAIN.

# Get Rid of Extra Spacing
with open("spider.txt") as file:
  for line in file:
    print(line.strip().upper()) # gets rid of line and makes everything upper case
# output:
# THE ITSY BITSY SPIDER CLIMBED UP THE WATERSPOUT.
# DOWN CAME THE RAIN
# AND WASHED THE SPIDER OUT.
# OUT CAME THE SUN
# AND DRIED UP ALL THE RAIN
# AND THE ITSY BITSY SPIDER CLIMBED UP THE SPOUT AGAIN.

# Read the File lines then close
file = open("spider.txt")
lines = file.readlines()
file.close()

# Read the File lines into LIST, then SORT, then close
file = open("spider.txt")
lines = file.readlines()
lines.sort()
print(lines)

# output:
# ['Down came the rain\n', 'Out came the sun\n', 'The itsy bitsy spider climbed up the waterspout.\n', 'and dried up all the rain\n', 'and the itsy bitsy spider climbed up the spout again.\n', 'and washed the spider out.\n']

# Writing Files
with open("novel.txt", "w") as file:
  file.write("It was a dark and stormy night")

# "w" = write, "r" = read but it's default so optional. 
# "w" mode means it can't read, there will be an error
# "w" mode means if I open a file that already exists, I will delete all old contents just by opening it in "w" mode, DOUBLE CHECK

# "x" = exclusive creation, but if file already exists, then this will fail
# "a" = writing, appending to end of file if it exists\
# "+" oepn for BOTH reading and writing

# OS MODULE
# Deleting a file, using Remove function
import os
  os.remove("novel.txt")
# note: cannot delete a file that doesn't exist

# Renaming a file with Rename function
# first parameter = old name, second parameter = new name
os.rename("first_draft.txt", "finished_masterpiece.txt")

# OS PATH SUB-MODULE
# Checking Existence of File and Directory
os.path.exists("finished_masterpiece.txt")
# output:
# True
os.path.exists("userlist.txt")
# output:
# False

# Checking Size of File
os.path.getsize("spider.txt")
# output:
# 192

# Check When Last Modified
os.path.getmtime("spider.txt")
# output:
# 1578322923.8999994
# timestamp (Unix timestamp) that represents number of seconds since January 1st, 1970

# Datetime Module: makes Unix timestamp readable
import datetime
timestamp = os.path.getmtime("spider.txt")
datetime.datetime.fromtimestamp(timestamp)
# output:
# datetime.datetime(2020, 1, 6, 7, 2, 3, 899999)

# Specifying Location of File with ABSPATH function
os.path.abspath("spider.txt")
# output:
# '/home/user/spider.txt'

# Checking Existence *Only for File, not Directory*
import os
file = "file.dat"
if os.path.isfile(file):
  print(os.path.isfile(file))
  print(os.path.getsize(file))
else:  
  print(os.path.isfile(file))
  print("File not found")
# output:
# False
# File not found

# DIRECTORIES
# Checking with Directory Your Program is Executing In, use getcwd method
print(os.getcwd())
# output:
# /home/user

# Creating a New Directory
os.mkdir("new_dir")

# Change Directories
os.chdir("new_dir")
  os.getcwd() # now check which directory
# output:
# '/home/user/new_dir'

# Remove Directory
os.mkdir("newer_dir")
  os.rmdir("newer_dir")

# Checking Contents of Files and Sub-directories of Directory in a LIST
os.listdir("website")
# output:
# [‘images’, ‘index.html’, ‘favicon.ico’]

# Check if String is Directory or File
os.path.isdir # to see if it's a directory
dir = "website" # defining variable with name of directory
  for name in os.listdir(dir): # iterate through file names
    fullname = os.path.join(dir, name) # join directory to each file name and create a String with a valid full name
    if os.path.isdir(fullname): # check if it's a directory or a file
      print("{} is a directory".format(fullname))
    else:
      print("{} is a file".format(fullname))
# output:
# website/images is a directory
# website/index.html is a file
# website/favicon.ico is a file

# Concatenate Directories
os.path.join()

