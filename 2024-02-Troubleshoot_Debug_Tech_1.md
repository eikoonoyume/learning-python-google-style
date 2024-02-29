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
