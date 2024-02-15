# Qwiklabs: Log analysis using regex
# log file = syslog.log, contains logs related to ticky
# view file
cat syslog.log
# output:
Jan 31 00:09:39 ubuntu.local ticky: INFO Created ticket [#4217] (username)
Jan 31 00:16:25 ubuntu.local ticky: INFO Closed ticket [#1754] (username)
Jan 31 00:21:30 ubuntu.local ticky: ERROR The ticket was modified while updating (username)
Jan 31 00:44:34 ubuntu.local ticky: ERROR Permission denied while closing ticket (username)

# find all logs from ticky 
grep ticky syslog.log

# output:
Jan 31 00:09:39 ubuntu.local ticky: INFO Created ticket [#4217] (mdouglas)
Jan 31 00:16:25 ubuntu.local ticky: INFO Closed ticket [#1754] (noel)
Jan 31 00:21:30 ubuntu.local ticky: ERROR The ticket was modified while updating (breee)
Jan 31 00:44:34 ubuntu.local ticky: ERROR Permission denied while closing ticket (ac)
Jan 31 01:00:50 ubuntu.local ticky: INFO Commented on ticket [#4709] (blossom)
Jan 31 01:29:16 ubuntu.local ticky: INFO Commented on ticket [#6518] (rr.robinson)
Jan 31 01:33:12 ubuntu.local ticky: ERROR Tried to add information to closed ticket (mcintosh)
Jan 31 01:43:10 ubuntu.local ticky: ERROR Tried to add information to closed ticket (jackowens)
Jan 31 01:49:29 ubuntu.local ticky: ERROR Tried to add information to closed ticket (mdouglas)
Jan 31 02:30:04 ubuntu.local ticky: ERROR Timeout while retrieving information (oren)

# search all ERROR logs
grep "ERROR" syslog.log

# output:
Jan 31 00:21:30 ubuntu.local ticky: ERROR The ticket was modified while updating (breee)
Jan 31 00:44:34 ubuntu.local ticky: ERROR Permission denied while closing ticket (ac)
Jan 31 01:33:12 ubuntu.local ticky: ERROR Tried to add information to closed ticket (mcintosh)
Jan 31 01:43:10 ubuntu.local ticky: ERROR Tried to add information to closed ticket (jackowens)
Jan 31 01:49:29 ubuntu.local ticky: ERROR Tried to add information to closed ticket (mdouglas)
Jan 31 02:30:04 ubuntu.local ticky: ERROR Timeout while retrieving information (oren)
Jan 31 02:55:31 ubuntu.local ticky: ERROR Ticket doesn't exist (xlg)
Jan 31 03:05:35 ubuntu.local ticky: ERROR Timeout while retrieving information (ahmed.miller)
Jan 31 03:08:55 ubuntu.local ticky: ERROR Ticket doesn't exist (blossom)
Jan 31 03:39:27 ubuntu.local ticky: ERROR The ticket was modified while updating (bpacheco)

# find specific ERROR message
grep "ERROR Tried to add information to closed ticket" syslog.log

# output:
Jan 31 01:33:12 ubuntu.local ticky: ERROR Tried to add information to closed ticket (mcintosh)
Jan 31 01:43:10 ubuntu.local ticky: ERROR Tried to add information to closed ticket (jackowens)
Jan 31 01:49:29 ubuntu.local ticky: ERROR Tried to add information to closed ticket (mdouglas)
Jan 31 04:31:49 ubuntu.local ticky: ERROR Tried to add information to closed ticket (oren)
Jan 31 05:12:39 ubuntu.local ticky: ERROR Tried to add information to closed ticket (oren)
Jan 31 05:18:45 ubuntu.local ticky: ERROR Tried to add information to closed ticket (sri)
Jan 31 08:01:40 ubuntu.local ticky: ERROR Tried to add information to closed ticket (jackowens)
Jan 31 09:04:27 ubuntu.local ticky: ERROR Tried to add information to closed ticket (noel)

# write regex using python3 interpreter
# open Python shell
python3
# Python Shell = Python Interactive Shell, used to execute Python command
# import regex module
import re
line = "May 27 11:45:40 ubuntu.local ticky: INFO: Created ticket [#1234] (username)"

# use search() method, match string stored in line variable
re.search(r"ticky: INFO: ([\w ]*) ", line)
# output: <re.Match object; span=(29, 57), match='ticky: INFO: Created ticket '>

line = "May 27 11:45:40 ubuntu.local ticky: ERROR: Error creating ticket [#1234] (username)"
# use search() method to match string in line variable
re.search(r"ticky: ERROR: ([\w ]*) ", line)
# output: <re.Match object; span=(29, 65), match='ticky: ERROR: Error creating ticket '>

# use Python to create a dictionary
fruit = {"oranges": 3, "apples": 5, "bananas": 7, "pears": 2}
# sorted function = sort items in dictionary
sorted(fruit.items())
# output: [('apples', 5), ('bananas', 7), ('oranges', 3), ('pears', 2)]

# operator module, sort dictionary using item's key
# pass itemgetter() function as argument  to sorted() function
# second element of tuple needs to be sorted, pass argument 0 to the itemgetter function
import operator
sorted(fruit.items(), key=operator.itemgetter(0))
# output: [('apples', 5), ('bananas', 7), ('oranges', 3), ('pears', 2)]

# sort dictionary based on its values
# pass argument 1 to itemgetter function in operator module
sorted(fruit.items(), key=operator.itemgetter(1))
# output: [('pears', 2), ('oranges', 3), ('apples', 5), ('bananas', 7)]

#  reverse order of sort using reverse parameter (takes boolean argument: reverse=True)
sorted(fruit.items(), key=operator.itemgetter(1), reverse=True)
# output: [('bananas', 7), ('apples', 5), ('oranges', 3), ('pears', 2)]

# csv_to_html.py file converts data in csv file to HTML file that has a table of data
# create new csv file
nano user_emails.csv

# add following data:
Full Name, Email Address
Blossom Gill, blossom@abc.edu
Hayes Delgado, nonummy@utnisia.com
Petra Jones, ac@abc.edu
Oleg Noel, noel@liberomauris.ca
Ahmed Miller, ahmed.miller@nequenonquam.co.uk
Macaulay Douglas, mdouglas@abc.edu
Aurora Grant, enim.non@abc.edu
Madison Mcintosh, mcintosh@nisiaenean.net
Montana Powell, montanap@semmagna.org
Rogan Robinson, rr.robinson@abc.edu
Simon Rivera, sri@abc.edu
Benedict Pacheco, bpacheco@abc.edu
Maisie Hendrix, mai.hendrix@abc.edu
Xaviera Gould, xlg@utnisia.net
Oren Rollins, oren@semmagna.com
Flavia Santiago, flavia@utnisia.net
Jackson Owens, jackowens@abc.edu
Britanni Humphrey, britanni@ut.net
Kirk Nixon, kirknixon@abc.edu
Bree Campbell, breee@utnisia.net

# ctrl-o, Enter, ctrl-x to save
# executable permission
sudo chmod +x csv_to_html.py

# generate a webpage to visualize data in user_emails.csv
# csv_to_html.py takes 2 arguments: csv file, location that would host HTML page
# give permission to write to directory that would host the HTML page
sudo chmod o+w /var/www/html

# run csv_to_html.py by passing 2 arguments: user_emails.csv and /var/www/html
# append name to the path with HTML extension
./csv_to_html.py user_emails.csv /var/www/html/<html-filename>.html
# <html-filename> can be replaced by something like new_user_emails

# navigate to /var/www/html directory
ls /var/www/html

# view HTML page by entering [linux-instance-external-IP]/[html-filename].html into any web-browser

# create script named ticky_check.py
# this script generates 2 different reports from internal ticketing system log file (syslog.log)
# report for ranking errors
# report for user usage statistics

# create python script named ticky_check.py
nano ticky_check.py

# add shebang line
#!/usr/bin/env python3
# import all Python modules needed
import re
import csv
import operator
# initialize 2 dictionaries (one for number of different errors, another to count number of entries for each user, splitting between INFO and ERROR)
error_messages={}
per_user={}
logfile=r"/home/student-03-c1bd9d32e5c/syslog.log"
pattern=r"(INFO|ERROR)([\w']+|[\w\[\]#']+)(\(\w+\)|\(\w+\.\w+\))$"
  # breakdown: either INFO or ERROR then capture group: alphanumeric, (what is ')
  # then | and 1+ appearances of capture group: alphanumeric, open bracket, close bracket, (what is #')
  # open parentheses, 1+ appearance of alphanumeric | open paretheses, 1+ alphanumeric, period, 1+ alphanumeric, close parentheses as ending
# parse through each log entry in syslog.log by iterating
with open(logfile, "r") as f:
  for line in f:
# first check if it matches INFO or ERROR message formats using regex
    result=re.search(pattern, line)
      if result is None:
        continue
# add one to corresponding value in per_user dictionary when successful match appears
# if error message, add one to corresponding entry in error dictionary
      if result.groups()[0]=="INFO":
        category=result.groups()[0]
        message=result.groups()[1]
        name=str(result.groups()[2])[1:-1]
        if name in per_user:
          user=per_user[name]
          user[category]+=1
        else:
          per_user[name]={'INFO':1, 'ERROR':0}
       if result.groups()=="ERROR":
         category=result.groups()[0]
         message=result.groups()[1]
         name=str(result.groups()[2])[1:-1]
         error_messages[message]=error_messages.get(message,0)+1
         if name in per_user:
           user=per_user[name]
           user[category]+=1
          else:
            per_user[name]={'INFO':0,'ERROR':1}
# sort both per_user and error dictionary before creating csv report files
# error dictionary = sorted by number of errors from most common to lease
# user dictionary = sorted by username
sorted_messages=[("Error","Count")]+sorted(error_messages.items(), key=operator.itemgetter(1), reverse=True)
  # sorted_messages=[("Error","Count")+sorted(error_messages.items(), key=lambda x:x[1], reverse=True)
sorted_users=[("Username", "INFO", "ERROR")]+sorted(per_user.items())[0:8]
  # sorted_users=[("Username", "INFO", "ERROR")]+sorted(per_user.items())
  
# insert column names as ("Error", "Count") at 0 index position of sorted error dictionary
# insert column names as ("Username", "INFO", "ERROR") at 0 index position of sorted per_user dictionary
# store sorted dictionaries in 2 different files: error_message.csv and user_statistics.csv
with open("error_message.csv", "w") as error_file:
  for line in sorted_messages:
    error_file.write("{}, {}\n".format(line[0], line[1]))
with open("user_statistics.csv", "w") as user_file:
  for line in sorted_users:
    if isinstance(line[1], dict):
      user_file.write("{}, {}, {}\n".format(line[0], line[1].get("INFO"), line[1].get("ERROR")))
    else:
      user_file.write("{}, {}, {}\n".format(line[0], line[1], line[2]))
# ctrl-o, Enter, ctrl-x

# executable permission to ticky_check.py
chmod +x ticky_check.py
# run
./ticky_check.py
# convert error_message.csv to HTML file
./csv_to_html.py error_message.csv /var/www/html<html-filename>.html
# <html-filename> can be something like new_error_message

# convert user_statistics.csv into html file
./csv_to_html.py user_statistics.csv /var/www/html/<html-filename>.html
# name can be new_user_statistics

