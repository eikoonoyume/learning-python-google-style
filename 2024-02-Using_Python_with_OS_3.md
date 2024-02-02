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
