# Regular Expressions = regex/regexp, search query for text expressed by string pattern
log = "July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade"
# extract process ID by using index method to find first square bracket
# to not capture square brackets, start with next index, include 5 more characters after
index = log.index("[")
print(log[index+1:index+6])
# output: 12345

# extract process ID in more robust fashion
# RE module: allows use of search function to find regex inside strings
import re
log = "July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade"
# r"\[(\d+)\]" = string enclosed in square brackets, followed by 1+ digits
regex = r"\[(\d+)\]"
# re.search() function searches string log for match to regex, returns a Match object that has group() method, or None. Group number returns by result[1]
result = re.search(regex, log)
print(result[1])
# output: 1234

# Basic Matching with grep
# command line tool grep = prints any line that matches query, powerful tool for applying regex
# find words inside /usr/share/dict/words file, this file is used by spell-checking programs to verify if words exists or not, one word per line
# looking for "thon" 
grep thon /usr/share/dict/words
# string passing through grep is CASE SENSITIVE, must match exactly
# use -i parameter to match regardless of case
grep -i python/usr/share/dict/words
# use ".", dot = wildcard that can be replaced by any character in the results
grep l.rts /usr/share/dict/words
# output: alerts, blurts, flirts
# caret/circumflex ^ = indicates beginning of line that regex should match
# dollar sign $ = indicates the end of the line
# look for all words that start with string fruit
grep ^fruit /usr/share/dict/words
# look for all words that end with cat
grep cat$ /usr/share/dict/words

# Simple Matching in Python
# use re module, call search function to look for "aza" pattern on string "plaza"
# r at beginning of pattern = rawstring: Python shouldn't interpret any special characters, just as is
# store the return as result variable
import re
result = re.search(r"aza", "plaza")
print(result)
# output: <_sre.SRE_Match object; span=(2, 5), match='aza'>
# output shows there is a match object, position in string that matched, actual matching string

import re
result = re.search(r"aza", "bazaar")
print(result)
# output: <_sre.SRE_Match object; span=(1, 4), match='aza'>

import re
result = re.search(r"aza", "maze")
print(result)
# output: None

# use circumflex^ X pattern in storage function
print(re.search(r"^x", "xenon"))
# output: <_sre.SRE_Match object; span=(0, 1), match='x'>
# output matched at beginning of line

# using dot. to match any character
import re
print(re.search(r"p.ng", "penguin"))
# output: <_sre.SRE_Match object; span=(0, 4), match='peng'>
# output shows matching string is "peng"

import re
print(re.search(r"p.ng", "clapping"))
print(re.search(r"p.ng", "sponge"))
# output: <_sre.SRE_Match object; span=(4, 8), match='ping'>
# output: <_sre.SRE_Match object; span=(1, 5), match='pong'>

# use re.IGNORECASE to  match no matter the case
import re
print(re.search(r"p.ng", "Pangaea", re.IGNORECASE))
# output: <_sre.SRE_Match object; span=(0, 4), match='Pang'>

# Check if text passed contains vowels a, e, and i with exactly one occurrence of any other character in between
impore re
def check_aei (text):
  result = re.search(r"a.e.i", text)
  return result != None
print(check_aei("academia"))
print(check_aei("aerial"))
print(check_aei("paramedic"))
# output: True
# output: False
# output: True

# Wildcards and Character Cases
# dot. = regex wildcard because it matches more than one character
# character classes = written inside [] to list character wanted
# [Pp] allows for both upper and lowercase p
import re
print(re.search(r"[Pp]ython", "Python"))
# output: <_sre.SRE_Match object; span=(0, 6), match='Python'>

# define range of characters using a dash- inside []
# [a-z] = lowercase a to lowercase z range
# [A-Z] = uppercase a to uppercase z range
# [0-9] = all digits range
# [a-zA-Z0-9] = all of the above
import re
print(re.search(r"[a-z]way", "The end of the highway"))
print(re.search(r"[a-z]way", "What a way to go"))
print(re.search("cloud[a-zA-Z0-9]", "cloudy"))
print(re.search("cloud[a-zA-Z0-9]", "cloud9))
# output: <_sre.SRE_Match object; span=(18, 22), match='hway'>
# output: None **there was a space in front of string so no match**
# output: <_sre.SRE_Match object; span=(0, 6), match='cloudy'>
# output: <_sre.SRE_Match object; span=(0, 6), match='cloud9'>

# match any characters that aren't in a group = [^]
import re
print(re.search(r"[^a-zA-Z]", "This is a sentences with spaces."))
# add space to this list of characters right after uppercase Z
print(re.search(r"[^a-zA-Z ]", "This is a sentence with spaces."))
# output: <_sre.SRE_Match object; span=(4, 5), match=' '>
# [^a-zA-Z] is an exclusion list, so the first thing not excluded was the first space
# output: <_sre.SRE_Match object; span=(30, 31), match='.'>
# space is now excluded, so now it matched the first period

# pipe symbol = match either one expression or another
print(re.search(r"cat|dog", "I like cats."))
print(re.search(r"cat|dog", "I love dogs!"))
print(re.search(r"cat|dog", "I like both dogs and cats."))
print(re.findall(r"cat|dog", "I like both dogs and cats."))
# output: <_sre.SRE_Match object; span=(7, 10), match='cat'>
# matches cat
# output: <_sre.SRE_Match object; span=(7, 10), match='dog'>
# matches dog
# output: <_sre.SRE_Match object; span=(12, 15), match='dog'>
# get the first one of cat or dog that appears, which is dog
# output: ['dog', 'cat']
# get all possible matches with findall function

# Check if text passed contains punctuation symbols: commas, periods, colons, semicolons, questions marks, exclamation points
import re
def check_punctuation (text):
  result = re.search(r"^a-zA-Z ]", text)
  return result != None
print(check_punctuation("This is a sentence that ends with a period."))
print(check_punctuation("This is a sentence fragment without a period"))
print(check_punctuation("Aren't regular expressions awesome?"))
print(check_punctuation("Wow! We're really picking up some steam now!"))
print(check_punctuation("End of the line"))
# output: True
# output: False
# output: True
# output: True
# output: False

# Repetition Qualifiers
# check how to match characters several times using repeated matches
# .* (dot star) expression matches any character repeated as many times as possible(including 0)
import re
print(re.search(r"Py.*n", "Pygmalion"))
# output: <_sre.SRE_Match object; span=(0, 9), match='Pygmalion'>

print(re.search(r"Py.*n", "Python Programming"))
# output: <_sre.SRE_Match object; span=(0, 17), match='Python Programmin'>
# * star takes as many characters as possible = "greedy"
# modify to make it less greedy

# modify to only match letters
print(re.search(r"Py[a-z]*n", "Python Programming"))
# output: <_sre.SRE_Match object; span=(0, 6), match='Python'>

# 0 is possible!!
print(re.search(r"Py[a-z]*n", "Pyn"))
# output: <_sre.SRE_Match object; span=(0, 3), match='Pyn'>

# two more repetition qualifiers and questions mark help make more complex expressions
# plus + character matches ONE or more occurences of the character before it
import re
print(re.search(r"o+l+", "goldfish"))
# output: <_sre.SRE_Match object; span=(1, 3), match='ol'>
# output had one occurrence of each o and l

print(re.search(r"o+l+", "woolly"))
# output: <_sre.SRE_Match object; span=(1, 5), match='ooll'>
# output had two of each o and l

print(re.search(r"o+l+", "boil"))
# output: None
# output had another character in between o and l, no match

# ? question mark = multiplier, means either ZERO or one occurrence of character before it
import re
print(re.search(r"p?each", "To each their own"))
# output: <_sre.SRE_Match object; span=(3, 7), match='each'>\
# output shows P wasn't present but the ? mark makes the P optional, still have match

print(re.search(r"p?each", "I like peaches"))
# output: <_sre.SRE_Match object; span=(7, 12), match='peach'>

# repeating_letter_a function checks if text passed includes letter "a" (upper/lower) at lease TWICE
example) repeating_letter_a("banana"
# output: True
example) repeating_letter_a("pineapple")
# output: False

import re
def repeating_letter_a(text):
  result = re.search(r"[Aa].*a", text)
  # characters before "a" we want inside [] upper and lower [Aa], dot. for any character, *star wildcard
  return result != None
print(repeating_letter_a("banana")
print(repeating_letter_a("pineapple")
print(repeating_letter_a("Animal Kingdom"))
print(repeating_letter_a("A is for apple"))
# output: True
# output: False
# output: True
# output: True

# Escaping Characters
# when you want to match . because you need ., not using it as a wildcard
# Escape character \
import re
print(re.search(r".com", "welcome"))
# output: <_sre.SRE_Match object; span=(2, 6), match='lcom'> # didn’t escape, . = any character
# escape character version
print(re.search(r"\.com", "welcome"))
# output: None

print(re.search(r"\.com", "mydomain.com"))
# output: <_sre.SRE_Match object; span=(8, 12), match='.com'>
# \ backslash is confusing because sometimes it's used in defining special string characters
# use raw strings to avoid confusion with special characters
# \w = matches alphanumeric character, including letters, numbers, and underscores
import re
print(re.search(r"\w*", "This is an example"))
# output: <_sre.SRE_Match object; span=(0, 4), match='This'>
# output matches first four letters until the space, space is not part of the set of characters

print(re.search(r"\w*", "And_this_is_another"))
# output: <_sre.SRE_Match object; span=(0, 19), match='And_this_is_another'>

# \d = matching digits
# \s = matching whitespace characters like space, tab, new line
# \b = word boundaries
# regex101.com for more

# Check if text passed has at least 2 groups of alphanumberic characters separated by 1+ whitespace
# alphanumeric = letters, numbers, underscores
import re
def check_character_groups(text):
  result = re.search(r"\w\s+\w", text)
  # alphanum_ space with 1+ occurrence alphanum_
  return result != None
print(check_character_groups("One"))
print(check_character_groups("123  Ready Set GO"))
print(check_character_groups("username user_01"))
print(check_character_groups("shopping_list: milk, bread, eggs."))
# output: False
# output: True
# output: True
# output: False


# Regular Expressions in Action
# combine special characters to create patterns to match text we want
# list of countries - check which names start and end in a: A.*a
import re
print(re.search(r"A.*a", "Argentina"))
# output: <_sre.SRE_Match object; span=(0, 9), match='Argentina'>

print(re.search(r"A.*a", "Azerbaijan"))
# output: <_sre.SRE_Match object; span=(0, 9), match='Azerbaija'>
# output shows that we didn't specify we wanted pattern to match whole string
# make it stricter by adding beginning of line and end of line characters
print(re.search(r"^A.*a$", "Australia"))
# output: <_sre.SRE_Match object; span=(0, 9), match='Australia'>

# Validating Pattern
# start with letter and circumflex^, character class of all lower and upper and underscore: ^[a-zA-Z_]
# the rest of variable = as many numbers, letters, underscores that we want: [a-zA-Z0-9_]*$
import re
pattern = r"^[a-zA-Z_][a-zA-Z0-9_]*$"
print(re.search(pattern, "_this_is_a_valid_variable_name"))
# output: <_sre.SRE_Match object; span=(0, 30), match='_this_is_a_valid_variable_name'>

print(re.search(pattern, "this isn't a valid variable"))
# output: None
# output may not have spaces, doesn't match pattern

print(re.search(pattern, "my_variable1"))
# output: <_sre.SRE_Match object; span=(0, 12), match='my_variable1'>

print(re.search(pattern, "2my_variable1"))
# output: None
# output may not start with a number according to our pattern

# Check if the text passed looks like a standard sentence with uppercase first
# some lowercase letters or space after
# ending with punctuation
import re
def check_sentence(text):
  result = re.search(r"^[A-Z]+[A-Za-z\s]+[\.!\?]$", text)
  return result != None
print(check_sentence("Is this is a sentence?"))
print(check_sentence("is this is a sentence?"))
print(check_sentence("Hello"))
print(check_sentence("1-2-3-GO!"))
print(check_sentence("A star is born."))
# output: True
# output: False (not capital at beginning)
# output: False (no punctuation)
# output: False (no dashes in pattern)
# output: True

# Capturing Groups
# capturing groups = portions of pattern, enclosed in parenthesis
# taking info we matches and use it for something else
# list of full names stored as last name, comma, first name
# turn the name around, create a string that is first name last name
# create matching pattern that matches group of letters, comma, space, group of letters
import re
result = re.search(r"^(\w*), (\w*)$", "Lovelace, Ada")
print(result)
# output: <_sre.SRE_Match object; span=(0, 13), match='Lovelace, Ada'>
# output: ('Lovelace', 'Ada')
# \w = match letters, numbers, underscores

# look at output, use group method to return a tuple of two elements
print(result.groups())
# output: Lovelace, Ada

# can use indexing to access these groups
# first element = text matched by regex
# look at element at index 0
print(result[0])
# output: Lovelace

print(result[1])
# output: Ada

# construct final name using indexes
print(result[2])
"{} {}".format(result[2], result[1])
# output: Ada Lovelace

# rearrange_name function
# receives name by parameter
# search string with same pattern as before
import re
def rearrange_name(name):
  result = re.search(r"^(\w*), (\w*)$", name)
# if result = none, return as it is
  if result is None:
    return name
  return "{} {}".format(result[2], result[1])
rearrange-name("Lovelace, Ada")
# output: Ada Lovelace

import re
def rearrange_name(name):
  result = re.search(r"^(\w*), (\w*)$", name)
  if result is None:
    return name
  return "{} {}".format(result[2], result[1])
rearrange_name("Ritchie, Dennis")
# output: Dennis Ritchie

# pattern currently won't match middle name initials
# add extra characters: add spaces and dots and dashes
import re
def rearrange_name(name):
  result = re.search(r"^([\w \.-]*), ([\w \.-]*)$", name)
  # after \w there is a space, then \.- for period and dash
  if result == None:
    return name
  return "{} {}".format(result[2], result[1])
rearrange_name("Hopper, Grace M.")
# output: Grace M. Hopper

# use rearrange_name function to match middle names, middle initials, double surnames
import re
def rearrange_name(name):
  result = re.search(r"^([\w \.-]*), ([\w \.-]*)$", name)
  if result == None:
    return name
  return "{} {}".format(result[2], result[1])
name=rearrange_name("Kennedy, John F.")
print(name)
# output: John F. Kennedy

# More Repetition Qualifiers
# find patterns that repeats a specific number of times
# numeric repetition qualifiers = {} with 1-2 numbers to specify range
# match any string of exactly 5 letters
import re
print(re.search(r"[a-zA-Z]{5}", "a ghost"))
# output: <_sre.SRE_Match object; span=(2, 7), match='ghost'>

# use findall function to find more matches
import re
print(re.findall(r"[a-zA-Z]{5}", "a scary ghost appeared"))
# output: ['scary', 'ghost', 'appea']

# match words that are exactly 5 letters long using \b at beginning and end
import re
re.findall(r"\b[a-zA-Z]{5}\b", "A scary ghost appeared"))
# output: ['scary', 'ghost']

# find words within range of 5 to 10 letters or numbers
import re
print(re.findall(r"\w{5,10}", "I really like strawberries"))
# output: ['really', 'strawberri']

# ranges can be open ended using number followed by comma 
# no upper boundary limit, only limit is maximum repetitions in source text
import re
print(re.findall(r"\w{5,}", "I really like strawberries"))
# output: ['really', 'strawberries']

# comma followed by number = zero up to that amount of repetitions
import re
print(re.search(r"s\w{.20}", "I really like strawberries"))
# s followed by up to 20 alphanumeric characters
# output: <_sre.SRE_Match object; span=(14, 26), match='strawberries'>

# long_words function
# returns all words that are at least 7 characters
import re
def long_words(text):
  pattern = r"\w{7,}"
  result = re.findall(pattern, text)
  return result
print(long_words("I like to drink coffee in the morning."))
print(long_words("I also have a taste for hot chocolate in the afternoon."))
print(long_words("I never drink tea late at night."))
# output: ['morning']
# output: ['chocolate', 'afternoon']
# output: []

# Extracting PID using regex in Python
# \ backslash = escape character then [ because we want it 
# \d+ for 1 or more numerical characters
# \ backslash then ] because we want it
import re
log = "July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade"
regex = r"\[(\d+)\]"
# call search function, capturing groups
result = re.search(regex, log)
# access the value at index 1
print(result[1])
# output: 12345

import re
log = "July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade"
regex = r"\[(\d+)\]"
result = re.search(regex, log)
result = re.search(regex, "A completely different string that also has numbers [34567]")
print(result[1])
# output: 34567

# what if the string doesn't have a block of numbers between square brackets?
import re
log = "July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade"
regex = r"\[(\d+)\]"
result = re.search(regex, log)
result = re.search(regex, "A commpletely different string that also has numbers [34567]")
result = re.search(regex, "99 elephants in a [cage]")
print(result[1])
# output: error

# extract_pid function
# extract the ID or PID
import re
log = "July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade"
regex = r"\[(\d+)\]"
result = re.serach(regex, log)
result = re.search(regex, "A completely different string that also has numbers [34567]")
result = re.search(regex, "99 elephants in a [cage]")
def extract_pid(log_line):
  regex = r"\[(\d+)\]"
# store the result of search function as result value
  result = re.search(regex, log_line)
# no success leads to None
  if result is None:
    return ""
# return empty string if PID wasn't found
  return result[1]
# access first capture group if match found
print(extract_pid(log))
# test function with original log line to check it does it right
# output: 12345

print(extract_pid("99 elephants in a [cage]"))
# output: ""

# Add to the regex in the extract_pid function, return uppercase message in parenthesis after the process id
import re
def extract_pid(log_line):
  regex = r"\[(\d+)\] \: ([A-Z]*)" 
  # \: followed by space
  # any number of capital letters [A-Z]*
  if result is None:
    return None
  return "{} ({})".format(result[1], result[2])
print(extract_pid("July 31 07:51:48 mycomputer bad_process[12345]: ERROR Performing package upgrade")) 
print(extract_pid("99 elephants in a [cage]")) 
print(extract_pid("A string that also has numbers [34567] but no uppercase message")) 
print(extract_pid("July 31 08:08:08 mycomputer new_process[67890]: RUNNING Performing backup"))
# output: 12345 [ERROR]
# output: None
# output: None
# output: 67890 (RUNNING)

# Splitting and Replacing
# Review: search function = re.search
# Review: find all function = re.findall
# New: split = (instead of taking string as separator) take any regex as separator = re.split
# split a piece of text into separate sentences, check for dots, question marks, exclamation marks
import re
re.split(r"[.?!]", "One sentence. Another one? And the last one!")
# no backslash escapes for any .?!, they are taken literally as .?! and not as special characters
# output: ['One sentence', ' Another one', ' And the last one', '']
# notation marks .?! are not in the resulting list
# include .?! using capturing parentheses
import re
re.split(r"([.?!])", "One sentence. Another one? And the last one!")
# output: ['One sentence', '.', ' Another one', '?', ' And the last one', '!', '']

# New: sub function used to create new strings by substituting all or part of them for a different string, similar to replace method = re.sub
# anonymize logs that have user email addresses by removing all email addresses
# part 1: part before the @ sign
# part 2: part after the @ sign
# include alphanumeric characters: \w (letters, numbers, underscore)
# include .%+-
# after @ sign, include only alphanumeric, . -
# redact anything that looks like an address
import re
re.sub(r"[\w.%+-]+@[\w.-]+", "[REDACTED]", "Received an email for go_nuts950@my.example.com")
# output: Received an email for [REDACTED]

# use sub to create new string, use parentheses to create capturing groups
# 1st parameter: expression that contains the two groups we want to match (before comma and after comma)
# 2nd parameter: replace the matching string
# Use \2 to indicate second captured group, follow with space and \1 to indicate 1st captured group
# capturing groups = \ and the number that indicates corresponding captured group
import re
re.sub(r"^([\w .-]*), ([\w .-]*)$", r"\2 \1", "Lovelace, Ada")
# output: Ada Lovelace

# Writing File Paths in Code
# code includes file path location for archived content
# another line/block of code tells where to specifically create a file and store info on it
# specific structure depends on OS
# Windows: start with drive name like C or D, uses \
# Mac and Linux: start with / (root or forward slash)
# Windows: C:\my-directory\target-file.txt
# Windows directory written in Python uses forward slash / (recommended): C:/my-directory/target-file.txt
# If using backslash for Windows directory in Python, it'll count as special character, so you use it twice every time: C:\\my-directory\\target-file.txt

# Using current directory = CWD command = os.getcwd()
# identify output holder to reference external files: outputs['current_directory_before'] = os.getcwd()
# list files and directories to find file path: outputs['files_and_directories']=os.listdir()
# access file paths for environment variables: outputs['path_value']=os.environ.get('PATH')

# Qwiklabs: Work with Regular Expressions
# navigate to directory
cd data
# list files to find data
ls
# view contents of user_emails.csv
cat user_emails.csv
# navigate to scripts directory
cd ~/scripts
# list contents of scripts directory
ls
# in script.py, use regex to find all instances of old doman ("abc.edu") in user_emails.csv, replace with new domain ("xyz.edu")
# update file's permissions
sudo chmod 777 script.py
# edit script.py
nano script.py
# import libraries for csv module
import csv
# import regex module
import re
# contains_domain function
# address and domain parameters, check if email address belongs to old domain
def contains_domain(address, domain):
  domain_pattern = r'[\w\.-]+@'+domain+'$'
    if re.match(domain_pattern, address):
      return True
    return False
# replace domain name
# replace_domain function
# make pattern that identifies sub-strings containing old domain, store pattern in variable old_domain_pattern
# use substitution function sub() to replace old name with new and return updated email address
def replace_domain(address, old_domain, new_domain):
  old_domain_pattern = r'' + old_domain + '$'
  address = re.sub(old_domain_pattern, new_domain, address)
  return address

# write CSV file with replaced domain
# declare old_domain and new_domain using main()
old_domain, new_domain = 'abc.edu', 'xyz.edu'
# store path of user_emails.csv in csv_file_location
# give file path for resulting updated list in variable report_file
csv_file_location = '/home/<username>/data/user_emails.csv
  report_file = '/home/<username>/data' + '/updated_user_emails.csv'
# initialize empty list to store user email addresses
# pass list to contains_domain, replace domains with replace_domain
user_email_list = []
  old_domain_email_list = []
  new_domain_email_list = []
# iterate over user_email_list, matches will be appended to list
# read data from list
    with open(csv_file_location, 'r') as f:
      user_data_list = list(csv.reader(f))
      user_email_list = [data[1].strip() for data in user_data_list[1:]]
# old_domain_email_list contains all email address with old domain, checked by contains_domain
      for email_address in user_email_list:
        if contains_domain(email_address, old_domain):
          old_domain_email_list.append(email_address)
          replaced_email = replace_domain(email_address, old_domain, new_domain)
          new_domain_email_list.append(replaced_email)
# define headers for output file through user_data_list
      email_key = ' ' + 'Email Address'
      email_index = user_data_list[0].index(email_key)
# replace email addresses in user_data_list by iterating over new_domain_email_list, replace values in user_data_list
# close file with close() method
      for user in user_data_list[1:]:
        for old_domain, new_domain in zip(old_domain_email_list, new_domain_email_list):
          if user[email_index] == ' ' + old_domain:
            user[email_index] = ' ' + new_domain
      f.close()
# write list to output file declared at beginning of script within variable report_file
  with open(report_file, 'w+') as output_file:
    writer = csv.wrtier(output_file)
    writer.writerows(user_data_list)
    output_file.close()
# call main() method
main()
# ctrl-o, enter ctrl-x to save
# run file
./script.py
# view newly generated file
ls ~/data
# view contents of file
cat ~/data/updated_user_emails.csv


