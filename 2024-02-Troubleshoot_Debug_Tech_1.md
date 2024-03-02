# Troubleshooting: Silently Crashing Application
# strace = tool to look more deeply at what program is doing, traces system calls made by program, tells us what call results were
strace ./purplebox.py

# system calls = calls programs running on our computer make to the running kernel

# less = command to scroll through a lot of texts
# go to end of file, press Shift G then scroll up to see
# strace -o flag = to store the output into file then browse contents of file, lets us refer back to file later

strace -o failure.strace ./purplebox.py
less failure.strace

# open at = system call name that opens files or directories
# there's a negative number after a return code = the directory doesn't exist

# create directory, start program again
mkdir ./config/purplebox
./purplebox.py

# this was an immediate remediation

# tools
# top = command to show the state of the computer and processes using the most CPU and see that the computer is super overloaded
# kill-stop = command to suspend the execution of the program until you let it continue or terminate
# ltrace = go through logs
# iotop = tool similar to top, lets us see which processes are using the most input and output
# iostat and vmstat = show statistics on the input/output operations and the virtual memory operations
# ionice = make our backup system reduce its priority to access the disk and let the web services use it too
# iftop = shows current traffice on network interfaces
# rsync -bwlimit = used to back up data with option to limit the bandwidth
# nice = command to reduce priority of the process and access the CPU

# Intermittently Failing Script
# run program
cd meeting_reminder
./meeting_reminder.sh

# date format issue
# open meeting_reminder.sh script with Bash
atom meeting_reminder.sh

#!/bin/bash

meeting_info=$(zenity --forms \
    --title 'Meeting' --text 'Reminder information' \
    --add-calendar 'Date' --add-entry 'Title' \
    --add-entry 'Emails' \
    2>/dev/null)
if [[ -n "$meeting_info" ]]; then
    python3 send_reminders.py "$meeting_info"
fi

# Zenity = application showing the window to select the date, title, emails
# variable where zenity ouput is stored = meeting_info
# parameters = send_reminders.py, Python3 script
# add echo statement
#!/bin/bash

meeting_info=$(zenity --forms \
    --title 'Meeting' --text 'Reminder information' \
    --add-calendar 'Date' --add-entry 'Title' \
    --add-entry 'Emails' \
    2>/dev/null)


echo $meeting_info

# save and run
./meeting_reminder.sh
# output:
01/13/2020|Test|amanda
Failure to send email


# open Python script, print better error
atom send_reminders.py

def main():
    if len(sys.argv) < 2:
        return usage()


    try:
        date, title, emails = sys.argv[1].split('|')
        message = message_template(date, title)
        send_message(message, emails)
        print("Successfully sent reminders to:", emails)
    except Exception as e:
        print("Failure to send email", file=sys.stderr)
    except Exception as e:
       print("Failure to send email: {}".format(e), file=sys.stderr)
# parameter received is split in three
# prepares message to be sent and sends it
# prints message when sent successfully
# error message we've already seen
# make error helpful by printing the exception that generated the failure

def main():
    if len(sys.argv) < 2:
        return usage()


    try:
        date, title, emails = sys.argv[1].split('|')
        message = message_template(date, title)
        send_message(message, emails)
        print("Successfully sent reminders to:", emails)
    except Exception as e:
        print("Failure to send email", file=sys.stderr)
    except Exception as e:
       print("Failure to send email: {}".format(e), file=sys.stderr)
# save and run
atom send_reminders.py
./meeting_reminder.sh
# output:
01/13/2020|Test|amanda
Failure to send email: time data ‘01/13/2020’ does not match format ‘%d/%m/%y’

# fix the date to make sure it gives the right date no matter the country/location
# use zenity parameter
atom meeting_reminder.sh

# change shell script to use --forms-date-format parameter, set to %Y-%m-%d for international standard date format
#!/bin/bash

meeting_info=$(zenity --forms \
    --title 'Meeting' --text 'Reminder information' \
    --add-calendar 'Date' --add-entry 'Title' \
    --add-entry 'Emails' \
    --forms-date-format='%Y-%m-%d' \
    2>/dev/null)
echo $meeting_info
if [[ -n "$meeting_info" ]]; then
    python3 send_reminders.py "$meeting_info"
fi

# go to Python script, change function to same date format
atom send_reminders.py


def dow(date):
    dateobj = datetime.datetime.strptime(date, r"%Y-%m-%d") # was %d/%m/%y before
    return dateobj.strftime("%A")

# save and run
./meeting_reminder.sh
# output:
2020-01-13|Test|amanda
Successfully sent reminders to: amanda


# The compare_strings function is supposed to compare just the alphanumeric content of two strings, ignoring upper vs lower case and punctuation. But something is not working. Fill in the code to try to find the problems, then fix the problems. Your goal is to search for the character “-” but - is typically reserved for ranges. Find a work-around that enables you to compare the two strings.

import re
def compare_strings(string1, string2):
 #Convert both strings to lowercase
 #and remove leading and trailing blanks
 string1 = string1.lower().strip()
 string2 = string2.lower().strip()




 #Ignore punctuation
 punctuation = r"[.?!,;:-']" # re.error: bad character range :-' at position 6








 string1 = re.sub(punctuation, r"", string1)
 string2 = re.sub(punctuation, r"", string2)




 #DEBUG CODE GOES HERE
 print(string1 == string2)




 return string1 == string2




print(compare_strings("Have a Great Day!", "Have a great day?")) # True
print(compare_strings("It's raining again.", "its raining, again")) # True
print(compare_strings("Learn to count: 1, 2, 3.", "Learn to count: one, two, three.")) # False
print(compare_strings("They found some body.", "They found somebody.")) # False

# Change to:
import re
def compare_strings(string1, string2):
 #Convert both strings to lowercase
 #and remove leading and trailing blanks
 string1 = string1.lower().strip()
 string2 = string2.lower().strip()




 #Ignore punctuation
 # punctuation = r"[.?!,;:-']" # re.error: bad character range :-' at position 6
 punctuation = r"[.?!,;:'-]"








 string1 = re.sub(punctuation, r"", string1)
 string2 = re.sub(punctuation, r"", string2)




 #DEBUG CODE GOES HERE
 """
 change r"[.?!,;:-']" with r"[.?!,;:'-]" in punctuatin variable because of pattern error (Character range is out of order ('-' pattern))
 """
 print(string1 == string2)




 return string1 == string2




print(compare_strings("Have a Great Day!", "Have a great day?")) # True
print(compare_strings("It's raining again.", "its raining, again")) # True
print(compare_strings("Learn to count: 1, 2, 3.", "Learn to count: one, two, three.")) # False
print(compare_strings("They found some body.", "They found somebody.")) # False


# The datetime module supplies classes for manipulating dates and times, and contains many types, objects, and methods. You've seen some of them used in the dow function, which returns the day of the week for a specific date. We'll use them again in the next_date function, which takes the date_string parameter in the format of "year-month-day", and uses the add_year function to calculate the next year that this date will occur (it's 4 years later for the 29th of February during Leap Year, and 1 year later for all other dates). Then it returns the value in the same format as it receives the date: "year-month-day".
# Can you find the error in the code? Is it in the next_date function or the add_year function? How can you determine if the add_year function returns what it's supposed to? Add debug lines as necessary to find the problems, then fix the code to work as indicated above. 
import datetime
from datetime import date


def add_year(date_obj):
  try:
    new_date_obj = date_obj.replace(year = date_obj.year + 1)
  except ValueError:
    # This gets executed when the above method fails, 
    # which means that we're making a Leap Year calculation
    new_date_obj = date_obj.replace(year = date_obj.year + 4)
  return new_date_obj


def next_date(date_string):
  # Convert the argument from string to date object
  date_obj = datetime.datetime.strptime(date_string, r"%Y-%m-%d")
  next_date_obj = add_year(date_obj)


  # Convert the datetime object to string, 
  # in the format of "yyyy-mm-dd"
  next_date_string = next_date_obj.strftime("yyyy-mm-dd")
  return next_date_string


today = date.today()  # Get today's date
print(next_date(str(today))) 
# Should return a year from today, unless today is Leap Day


print(next_date("2021-01-01")) # Should return 2022-01-01
print(next_date("2020-02-29")) # Should return 2024-02-29

# Change to:
import datetime
from datetime import date


def add_year(date_obj):
  try:
    new_date_obj = date_obj.replace(year = date_obj.year + 1)
  except ValueError:
    # This gets executed when the above method fails, 
    # which means that we're making a Leap Year calculation
    new_date_obj = date_obj.replace(year = date_obj.year + 4)
  return new_date_obj


def next_date(date_string):
  # Convert the argument from string to date object
  date_obj = datetime.datetime.strptime(date_string, r"%Y-%m-%d")
  next_date_obj = add_year(date_obj)


  # Convert the datetime object to string, 
  # in the format of "yyyy-mm-dd"
  next_date_string = next_date_obj.strftime("%Y-%m-%d")
  return next_date_string


today = date.today()  # Get today's date
print(next_date(str(today))) 
# Should return a year from today, unless today is Leap Day


print(next_date("2021-01-01")) # Should return 2022-01-01
print(next_date("2020-02-29")) # Should return 2024-02-29

# Applying Binary Search in Troubleshooting
# bisect = cut problem in half for each iteration
# example: identify cuase of failure by bisecting list of files
# create copy of directory with just 6 of total of 12 files, start program
# crash = bad file is among 6, no crash = it's the other 6
# pick 3 of the 6, run program, eventually figure out which of the files is the problem
# use bisect = Git command that receives 2 points in time in Git history and repeatedly lets us try the code at the middle point between them until we find the commit that caused breakage
# bisect commit lets you find out which command causes the software to stop working on computer if you're using open source software that's tracking Git


# Finding Invalid Data
# use --server flag = takes name of database server, pass test as the parameter
/import_data$ cat contacts.csv | ./import.py --server test
# output: Import error
# use wc = to count characters, words, and lines in a file
# use wc -l = prints the amount of lines in a file
/import_data$ we -l contacts.csv
# output: 100 contacts.csv

# use head = command to print the first lines in the file
# use tail = command to print the last lines of file
# pass amount of lines you want to include as parameter
# example: head -15 = first 15 lines, tail -20 = last 20 lines
/import_data$ head -15 contacts.csv
/import_data$ tail -20 contacts.csv

# use pipes to connect output of head or tail commands
/import_data$ head -50 contacts.csv | ./import.py --server test
# run program
# first half failes, split again
# use pipe to take only half of previous number
/import_data$ head -50 contacts.csv | head -25 | ./import.py --server test
# output: Import successful
# verify that failure exists here
# take first half using head, get second half using tail
/import_data$ head -50 contacts.csv | tail -25 | ./import.py --server test
# output: Import error
# split again
/import_data$ head -50 contacts.cvs | tail -25 | head -13 | ./import.py --server test
# output: Import successful
# split again
/import_data$ head -50 contacts.csv | tail -25 | tail -12 | head -6 | ./import.py --server test
# output: Import error
# split one more time
/import_data$ head -50 contacts.csv | tail -25 | tail -12 | head -6 | head -3 | ./import.py --server test
# output: Import error
# look at last 3 entries, problem: comma separated file but not written between quotes
# edit file and fix
atom cat contacts.csv
# run again
/import_data$ cat contacts.csv | ./import.py --server test


# The find_item function uses binary search to recursively locate an item in a list, returning True if found, False otherwise. Something is missing from this function. Can you spot what it is and fix it? Add debug lines where appropriate to help you narrow down the problem.

def find_item(list, item):
    # Returns True if the item is in the list, False if not.
    if len(list) == 0:
        return False
    # Is the item in the center of the list?
    middle = len(list)//2
    if list[middle] == item:
        return True
    # Is the item in the first half of the list?
    if item < list[middle]:
        # Call the function with the first half of the list
        return find_item(list[:middle], item)
    else:
        # Call the function with the second half of the list
        return find_item(list[middle+1:], item)

    return False
    
#Do not edit below this line - This code helps check your work!
list_of_names = ["Parker", "Drew", "Cameron", "Logan", "Alex", "Chris", "Terry", "Jamie", "Jordan", "Taylor"]

print(find_item(list_of_names, "Alex")) # True
print(find_item(list_of_names, "Andrew")) # False
print(find_item(list_of_names, "Drew")) # True
print(find_item(list_of_names, "Jared")) # False

# Fix to:
def find_item(list, item):
    list.sort()
    # Returns True if the item is in the list, False if not.
    if len(list) == 0:
        return False
    # Is the item in the center of the list?
    middle = len(list)//2
    if list[middle] == item:
        return True
    # Is the item in the first half of the list?
    if item in list[:middle]:
        # Call the function with the first half of the list
        return find_item(list[:middle], item)
    else:
        # Call the function with the second half of the list
        return find_item(list[middle+1:], item)
    return False
#Do not edit below this line - This code helps check your work!
list_of_names = ["Parker", "Drew", "Cameron", "Logan", "Alex", "Chris", "Terry", "Jamie", "Jordan", "Taylor"]

print(find_item(list_of_names, "Alex")) # True
print(find_item(list_of_names, "Andrew")) # False
print(find_item(list_of_names, "Drew")) # True
print(find_item(list_of_names, "Jared")) # False

# The binary_search function returns the position of key in the list if found, or -1 if not found. You want to make sure that it's working correctly, so you need to place debugging lines to let you know each time that the list is cut in half, whether you're on the left or the right. You do not want to print anything when the key is located.
# For example, the command binary_search([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 3) performs these steps:
# It determines that the key, 3, is in the left half of the list and prints "Checking the left side". 
# It then determines that 3 is in the right half of the new list and prints "Checking the right side".
# Finally, it returns the value of 2, which is the position of the key in the list.
# Add commands to the code to print out "Checking the left side" or "Checking the right side" in the appropriate places.

def binary_search(list, key):
    list.sort() # binary searh starts with a sorted list
    left = 0 # the first value of the list
    right = len(list) - 1 # the last value of the list

    while left <= right:
        middle = (left + right) //2

        if list[middle] == key:
            print("Middle elment")
            return middle
        elif list[middle] > key:
            # add debug statement here
            right = middle -1
        else:
            # add debug statement here
            left = middle +1
    return -1
print(binary_search([10, 2, 9, 6, 7, 1, 5, 3, 4, 8], 1))

print(binary_search([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 5))    

# fix to:
def binary_search(list, key):
    list.sort() # binary search starts with a sorted list
    left = 0 # the first value of the list
    right = len(list -1 # last value of the list

    while left <= right:
        middle = (left + right)//2
        
        if list[middle] == key:
            print("Middle element")
            return middle
        elif list[middle] > key:
            # add debug statement here
            print("Checking the left side")
            right = middle -1
        else:
            # add debug statement here
            print("Checking the right side")
            left = middle +1
    return -1
print(binary_search([10, 2, 9, 6, 7, 1, 5, 3, 4, 8], 1))

print(binary_search([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 5))    


# The best_search function compares linear_search and binary_search functions to locate a key in the list, returns how many steps each method took, and which method is the best for that situation. The list does not need to be sorted, as the binary_search function sorts it before proceeding (and uses one step to do so). Here, linear_search and binary_search functions both return the number of steps that it took to either locate the key or determine that it's not in the list. If the number of steps is the same for both methods (including the extra step for sorting in binary_search), then the result is a tie. Fill in the blanks to make this work.
def linear_search(list, key):
   #Returns the number of steps to determine if key is in the list

   #Initialize the counter of steps
   steps=0
   for i, item in enumerate(list):
       steps += 1
       if item == key:
           break
   return ___

def binary_search(list, key):
   #Returns the number of steps to determine if key is in the list
   
   #List must be sorted:
   list.sort()

   #The Sort was 1 step, so initialize the counter of steps to 1
   steps=1

   left = 0
   right = len(list) - 1
   while left <= right:
       steps += 1
       middle = (left + right) // 2
      
       if list[middle] == key:
           break
       if list[middle] > key:
           right = middle - 1
       if list[middle] < key:
           left = middle + 1
   return ___


def best_search(list, key):
   steps_linear = ___
   steps_binary = ___
   results = "Linear: " + str(steps_linear) + " steps, "
   results += "Binary: " + str(steps_binary) + " steps. "
   if (___):
       results += "Best Search is Linear."
   elif (___):
       results += "Best Search is Binary."
   else:
       results += "Result is a Tie."

   return results

print(best_search([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 1))
#Should be: Linear: 1 steps, Binary: 4 steps. Best Search is Linear.

print(best_search([10, 2, 9, 1, 7, 5, 3, 4, 6, 8], 1))
#Should be: Linear: 4 steps, Binary: 4 steps. Result is a Tie.

print(best_search([10, 9, 8, 7, 6, 5, 4, 3, 2, 1], 7))
#Should be: Linear: 4 steps, Binary: 5 steps. Best Search is Linear.

print(best_search([1, 3, 5, 7, 9, 10, 2, 4, 6, 8], 10))
#Should be: Linear: 6 steps, Binary: 5 steps. Best Search is Binary.

print(best_search([5, 1, 8, 2, 4, 10, 7, 6, 3, 9], 11))
#Should be: Linear: 10 steps, Binary: 5 steps. Best Search is Binary.

# fix to:
def linear_search(list, key):
    # returns the nummber of step  to determine if key is in the list
    
    # intialize new counter of steps
    steps=0
    for i, item in enumerate(list):
        steps += 1
        if item == key:
            break
    return steps
    
def binary_search(list, key):
    # returns the number of steps to determine if the key is in the list
    
    # list must be sorted
    list.sort()
    # the sort was 1 step, so initialize the counter of steps to 1
    steps=1
    
    left = 0
    right = len(list) -1
    while left <= right:
        steps += 1
        middle = (left + right)//2
        
        if list[middle] == key:
            break
        if list[middle] > key:
            right = middle -1
        if list[middle] < key:
            left = middle +1
    return steps
    
def best_search(list, key):
    steps_linear = linear_search(list, key)
    steps_binary = binary_search(list, key)
    results = "Linear: " + str(steps_linear) + " steps, "
    results += "Binary: " + str(steps_binary) + " steps. "
    if (steps_linear < steps_binary):
        results += "Best Seach is Linear."
    elif (steps_linear > steps_binary):
        results += "Best Sarch is Binary."
    else:
        results += "Result is a Tie
        
    return results
print(best_search([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 1))
#Should be: Linear: 1 steps, Binary: 4 steps. Best Search is Linear.

print(best_search([10, 2, 9, 1, 7, 5, 3, 4, 6, 8], 1))
#Should be: Linear: 4 steps, Binary: 4 steps. Result is a Tie.

print(best_search([10, 9, 8, 7, 6, 5, 4, 3, 2, 1], 7))
#Should be: Linear: 4 steps, Binary: 5 steps. Best Search is Linear.

print(best_search([1, 3, 5, 7, 9, 10, 2, 4, 6, 8], 10))
#Should be: Linear: 6 steps, Binary: 5 steps. Best Search is Binary.

print(best_search([5, 1, 8, 2, 4, 10, 7, 6, 3, 9], 11))
#Should be: Linear: 10 steps, Binary: 5 steps. Best Search is Binary.

# Qwiklabs: Debug Python scripts
# navigate to scripts directory
cd ~/scripts
# list all files
ls
# output: greetings.py
# view contents
cat greetings.py
# update file permissions
sudo chmod 777 greetings.py
# reproduce error by running file
./greetings.py
# enter name at prompt
# error = can't concatenate string and integer
# print statements that causes error
print("hello" + name + ", your random number is " + number)
# edit file with nano
nano greetings.py
# replace print statement
print("hello " + name + ", your random number is " + str(number))
# run
./greetings.py
# enter your name at prompt
# correct output appears

