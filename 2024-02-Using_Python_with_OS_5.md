# Manual Testing and Automated Testing
# software testing = process of evaluating computer code to determine whether or not it what's expected
# good tests help us catch mistakes, errors, bugs
# most basic way of testing = run it with different parameters, see if it returns expected values
# another manual test = using interpreter to try code before putting it in script
# formal software testing = codifies tests into its own software
# automatic testing = automate the process of checking if returned values is what's expected
# automatic testing = code written to test
# the more test cases (possible values), the better tested the code is, more guarantee that it does what's expected
# unittest = provides developers with set of tools to construct and run tests
# unittest can be run on individual components or by isolating units of code
# test fixture = preparing to perform 1+ tests, includes any action involved in testing cleanup (creating temporary or proxy databases/directories, starting a server process)
# test case = individual unit of testing that looks for a specific response to set of inputs
# TestCase = base class provided by unittest, used to create new test cases
# test suite = collection of test cases, test suites, or combo of both, used to compile tests that should be executed together
# test runner = runs the test, provides developers with outcome's data, use different interfaces (graphical, textual, etc), provide special value to developers to communiccate test results

# Use case
# define cake factor class and methods
from typing import List

class CakeFactory:
  def __init__(self, cake_type: str, size: str):
    self.cake_type = cake_type
    self.size = size
    self.toppings = []
    # price based on cake type and size
    self.price = 10 if self.cake_type == "chocolate" else 8
    self.price += 2 if self.size == "medium" else 4 if self.size == "large" else 0
    
  def add_topping(self, topping: str):
    self.toppings.append(topping)
    # adding 1 to the price for each topping
    self.price += 1
    
  def check_ingredients(self) -> List[str]:
    ingredients = ['flour', 'sugar', 'eggs']
    ingredients.append('cocoa') if self.cake_type =="chocolate" else ingredients.append('vanilla extract')
    ingredients += self.toppings
    return ingredients
    
  def check_price(self) -> float:
    return self.price
    
# example of creating a cake and adding toppings
cake = CakeFactory("chocolate", "medium")
cake.add_topping("sprinkles")
cake.add_topping("cherries")
cake_ingredients = cake.check_ingredients()
cake_price = cake.check_price()
cake_ingredients, cake_price
# output: (['flour', 'sugar', 'eggs', 'cocoa', 'sprinkles', 'cherries'], 14)


# define unittest methods
# test suite includes tests for cake's flavor, size, toppings, ingredients, price
# 1st test case = intentionally provide wrong value
# create specific statements to make sure program is behaving as should
# include providing incorrect data to determing if program will provide failed results
# encapsulate statements into test methods because unittest is class-based
import unittest

class TestCakeFactory(unittest.TestCase):
  def test_create_cake(self):
    cake = CakeFactory("vanilla", "small")
    self.assertEqual(cake.cake_type, "vanilla")
    self.assertEqual(cake.size, "small")
    self.assertEqual(cake.price, 8)
    
  def test_add_topping(self):
    cake = CakeFactory("chocolate", "large")
    cake.add_topping("sprinkles")
    self.assertIn("sprinkles", cake.toppings)

  def test_check_ingredients(self):
    cake = CakeFactory("chocolate", "medium")
    cake.add_topping("cherries")
    ingredients = cake.check_ingredients()
    self.assertIn("cocoa", ingredients)
    self.assertIn("cherries", ingredients)
    self.assertNotIn("vanilla extract", ingredients)

  def test_check_price(self):
    cake = CakeFactory("vanilla", "large")
    cake.add_topping("sprinkles")
    cake.add_topping("cherries")
    price = cake.check_price()
    self.assertEqual(price, 13)

# run unittests  unittest.TextTestRunner().run(unittest.TestLoader().loadTestsFromTestCase(TestCakeFactory))
# output: Ran 4 tests in 0.009s
# output: FAILED (failures=1)
# result = <unittest.runner.TextTestResult run=4 errors=0 failures=1>
# TextTestRunner() method = returns arunner (TextTestResult)
# 1 failure occurred, so self.assertEqual(price, 13) = incorrect because it's 14
# correct it so that it's 14
# fixing test_check_price()
class TestCakeFactory(unittest.TestCase):
# other tests remain the same
  def test_check_price(self):
    cake = CakeFactory("vanilla", "large")
    cake.add_topping("sprinkles")
    cake.add_topping("cherries")
    price = cake.check_price()
    self.assertEqual(price, 14)
# re-run
unittest.TextTestRunner().run(unittest.TestLoader().loadTestsFromTestCase(TestCakeFactory))
# output: Ran 1 test in 0.004s
# output: OK
# result = <unittest.runner.TextTestResult run=1 errors=0 failures=0>

# pytest
# pytest = Python testing tool, assists programmers in writing more effective and stable programs
# helps simplify process of writing, organizing, executing tests
# used to write a variety of tests including: integration, end-to-end, functional tests
# supports automatic test discovery
# generates informative test reports
# written with functions that use operation assert()
# assert() = commonly used debugging tool in Python, allows to include sanity checks
# false indicates a bug in code, exepction is raised, and halt of the program's execution
def divid(a, b):
  assert b != 0, "Cannot divide by zero"
  return a / b
# AssertionError message = informs programmer that a value cannot be divisable by zero

# pytest fixtures
# fixtures = separate parts of code that only run for tests, reusable pieces of test setups and teardown code
import pytest
class Fruit:
  def __init__(self, name):
    self.name = name
    self.cubed = False
  def cube(self):
    self.cubed = True
class FruitSalad:
  def __init__(self, *fruit_bowl):
    self.fruit = fruit_bowl
    self._cube_fruit()
  def _cube_fruit(self):
    for fruit in self.fruit:
      fruit.cube()
# Arrange
@pytest.fixture
def fruit_bowl():
  return [Fruit("apple"), Fruit("banana")]
def test_fruit_salad(fruit_bowl):
  # Act
  fruit_salad = FruitSalad(*fruit_bowl)
  # Assert
  assert all(fruit.cubed for fruit in fruit_salad.fruit)
# test_fruit_salad requests fruit_bowl
# pytest recognizes this, executes fruit_bowl fixture function, takes object it returns into test_fruit_salad as fruit_bowl argument

# unittest vs pytest
# unittest = directly in python, automatically detects test cases within an application, must be called from command line
# unittest = object-oriented approach to write tests, provide special assert methods like assertEqual(), assertTrue()
# pytest = imported from outside, automatically performed using prefix test_
# pytest = built-in assert statements to make tests easier to read and write

# Unit Test
# unit test = common type of automatic testing
# used to verify that small isolated parts of program are correct
# generally written alongside the code to test behavior of individual pieces or units like functions/methods
# isolation = important characteristic of unit test
# unit tests should only test the unit of code they target, the function or method that's being tested
# code
#!/usr/bin/env python3
import re
def rearrange_name(name):
  result = re.search(r"^([\w .]*), ([\w .]*)$", name)
  return "{} {}".format(result[2], result[1])
from rearrange import rearrange_name

rearrange_name("Lovelace, Ada")

# test
#!/usr/bin/env python3
import re
def rearrange_name(name):
  result = re.search(r"^([\w .]*), ([\w .]*)$", name)
  return "{} {}".format(result[2], result[1])
from rearrange import rearrange_name
python3
from rearrange import rearrange_name
rearrange_name("Lovelace, Ada")
# output: 'Ada Lovelace'
# used from keyword
# rearrange = module name that contains rearrange_name function

# Writing Unit Tests in Python
# write code that runs a test and verifies output
# create unit tests for rearrange_name function
#!/usr/bin/env python3
import re

def rearrange_name(name):
  result = re.search(r"^([\w .]*), ([\w .]*)$", name)
  return "{} {}".format(result[2], result[1])
# call script with same name of module that it's testing and appending suffix _test
# create the rearrange_test.py file
# test the rearrange_name function of rearrange module
# import unittest module
#!/usr/bin/env python3

import unittest

from rearrange import rearrange_name
# create our own class by inherits from test case
# write own test rearrange class that inherits from test case
# called test class, TestRearrange, indicated that it should inherit functionality from TestCase class in unit test module
class TestRearrange(unittest.TestCase):
# any methods defined in TestRearrange class that start with prefix test_ will automatically become tests
# turn manual test into automatic test that verifies that basic names are formatted correctly
# test_basic method, set up expected inputs and outputs, use assertEqual method provided by inherited test case class to verify we get what we expected
# assertEqual method basically says both arguments are equal, True = test passes, False = test fails with error printed
 def test_basic(self):
    testcase = "Lovelace, Ada"
    expected = "Ada Lovelace"
    self.assertEqual(rearrange_name(testcase), expected)
# run with unittest.main() function
unittest.main()
# make script executable and run
chmod +x rearrange_test.py
./rearrange_test.py
# output: Ran 1 test in 0.000s
# output: OK

# Edge Cases
# edge cases = inputs to code that produce unexpected results, found at extreme ends of ranges
# usually need special handling to behave correctly
# add test for empty string
def test_empty(self):
  testcase = ""
  expected = ""
  self.assertEqual(rearrange_name(testcase), expected)

./rearrange_test.py
# output: *error messager*

# perform simple check of result variable before operating
#!/usr/bin/env python3
import re
def rearrange_name(name):
  result = re.search(r"^([\w .-]*), ([\w .-]*)$", name)
  if result is None:
    return ""
  return "{} {}".format(result[2], result[1])
# norma input produces result, empty string returns original empty string
# sometimes you want the program to crash with error because it's bad for automation to fail silently
# other kinds of edge cases: passing zero to function that expects number; negative numbers; extremely large numbers
# run
./rearrange_test.py
# output: Ran 2 tests in 0.000s
# output: OK

# Additional Test Cases
from rearrange import rearrange_name
import unittest

class TestRearrange(unittest.TestCase):
  def test_basic(self):
    testcase = "Lovelace, Ada"
    expected = "Ada Lovelace"
    self.assertEqual(rearrange_name(testcase), expected)
  def test_empty(self):
    testcase = ""
    expected = ""
    self.assertEqual(rearrange_name(testcase, expected)
  def test_one_name(self):
    testcase = "Voltaire"
    expected = "Voltaire"
    self.assertEqual(rearrange_name(testcase), expected)
# run
unittest.main()
# case = Hopper, Grace M.
# output: Ran 3 tests in 0.000s
# output: OK
# case = Voltaire
# output: *error message*
# case = name without a comma, result is none so it should return empty string
# fix to returning original name instead of empty string
import re
def rearrange_name(name):
  result = re.search(r"^([\w .]*), ([\w .]*)$", name)
  if result is None:
    return name
  return "{} {}".format(result[2], result[1]_
# run
./rearrange_test.py
# output: Ran 4 tests in 0.000s
# output: OK

# White Box Test = clear-box test/transparent test
# relies on test creator's knowledge of software being tested to construct test cses
# test creator knows how the code works, can write test cases that use the understanding to make sure performances are as is expected
# helpful = test writer can use knowledge of source code to create tests that cover most behaviors
# unit tests run alongside or after code has been developed = white-box test because test cases are made with knowledge of how software works

# Black Box test = software being tested is treated like an opaque box
# test doesn't know what the internals of how software works
# written with awareness of what program should do, it's requirements or specifications, but now how it does it
# example: verify that when you type www.google.de in broswer, the search page returned is German
# don't know how servers process your request, but the end result is as expected
# useful = don't rely on knowledge of how system works, test cases = less biased by code
# usually cover situations not anticipated by programmar who originally wrote script

# Black-box White-box Combo
# black-box = verifies that details are displayed for products
# white-box = call different functions used by the page, checking that prices are displayed in the right currency, description is correctly wrapped, etc

# Integration tests = verify that instructions between different pieces of code in integrated environments work as expected
# verify interactions and make sure the whole system works expectly
# goal = take interactions and make sure the whole system works as expected
# usually take individual modules of code that unit tests verify then combine them into group to test
# might need to create a separate test environment
# usually take a bit more work to set up

# Regression tests = type of unit test, usually written as part of a debugging and troubleshooting process to verify that issue or error has been fixed once identified
# if script has bug, first trigger the buggy behavior, then fix the bug so that a test passes
# useful part of a test suite
# ensure that same mistakes don't happen twice
# same bug can't be reintroduced to code because then the regression test would fail

# Smoke tests = build verification tests
# serves as a kind of sanity check to find major bugs in programs
# answer basic questions like does the program run
# run before more refined testing takes place
# webservice example: smoke test = check if service is running on corresponding port

# load tests = verify that system behaves well when it's under significant load
# need to generate traffic to the application, simulating typical usage of service
# helpful when deploying new versions of applications, verifying that performance doesn't degrade
# test suite = taking together a group of tests of one or many kinds

# Test-Driven Development = TTD
# process that calls for creating test before writing the code
# creating some tests first makes sure that you've thought about the problem you're trying to solve, and some different approaches that you might use to accomplish it
# helps you think about the ways your program could fail and break, leading to valuable insights and change of approach
# involves first writing, then running to make it fail
# after verifying failure, write code that will satisfy the test, run again
# fail, then debug, run, repeat cycle for each new feature

# Try-Except Construct
# TypeError
# IndexError
# ValueError = type of error, indicates that there was a problem with one of the values of the parameters

# try-except construct 
#!/usr/bin/env python3

def character_frequency(filename):
  # counts frequency of each character in file
  # try to open file
  try:
    f = open(filename)
  except OSError:
    return None
  # process file
  characters = {}
  for line in f:
    for char in line:
      characters[char] = characters.get(char, 0) + 1
  f.close()
  return characters
# character_frequency() function = reads contents of file to count frequency of each character in them
# try-except block: first try to do operation, if error, then except block which matches error and cleans up
# be aware of errors that may be raised by functions called

# Raising Errors
# usually happens when some conditions necessary for function to properly function doen't do their job and return None/other base value
# function that verifies whether chosen username is valid
# check that name is at least certain amount of characters

def validate_user(username, minlen):
  if len(username) < minlen:
    return False
  if not username.isalnum():
    return False
  return True
# checking that username variable has least minlen characters
#!/usr/bin/env python3

def validate_user(username, minlen):
  if minlen < 1:
    raise ValueError("minlen must be at least 1")
  if len(username) < minlen:
    return False
  if not username.isalnum():
    return False
  return True
# keyword to generate at error = raise
from validations import validate_user
validate_user("", -1)
# output: *error message*
# call it with valued parameters
from validations import validate_user
validate_user("", 1)
# output: False
validate_user("myuser", 1)
# output: True

from validations import validate_user
validate_user(88, 1)
# output: *error message*

from validations import validate_user
validate_user ([], 1)
# output: *error message*

from validations import validate_user
validate_user(["name"], 1)
# output: *error message*

# assert keyword = tries to verify that a conditional expression is True, false = raises an assertion error with message
# helpful for debugging, add them in at any point
# get removed if interpreter is asked, in order to optimize faster running
# use raise to check for conditions that we expect
# use assert to verify situations that aren't expected, but might cause misbehavior
#!/usr/bin/env python3
 def validate_user(username, minlen):
   assert type(username) == str, "username must be a string"
   if minlen < 1:
     raise ValueError("minlen must be at least 1")
  if len(username) < minlen:
    return False
  if not username.isalnum():
    return False
  return True
# close interpreter, restart and import modified module

# Testing for Expected Errors
  from validations import validate_user
    validate_user([3], 1)
# output: *error message*
# negative value of minlen example: expectation is that it will raise an error
# use assert raises method
#!/usr/bin/env python3
  import unittest
  from validatesion import validate_user
  class TestValidateUser(unittest.TestCase):
    def test_valid(self)"
      self.assertEqual(validate_user("validuser", 3), True)
    def test_too_short(self):
      self.assertEqual(validate_user("inv", 5), False)
    def test_invalid_characters(self):
      self.assertEqual(validate_user*("invalid_user", 1), False)
    def test_invalid_minlen(self):
      self.assertRaises(ValueError, validate_user, "user", -1)
# run
  unittest.main()
# run
  ./validations_test.py
# output: Ran 4 tests in 0.000s
# output: OK

# Qwiklabs: Implementing unit testing
# add a test to reproduce the bug, make the necessary corrections, and verify that all the tests pass to make sure the script works

# navigate to data directory
  cd ~/data
# list files
  ls
# view contents of user_emails.csv
  cat user_emails.csv
# navigate to script directory
  cd ~/scripts
# list contents
  ls
# view emails.py
  cat emails.py

# populate_dictionary(filename) = reads user_emails.csv file, populates dictionary with name/value pairs
# find_emails(argv) = searchs dictionary created in previous function, returns associated email address
# sys.argv = where arguments from command line are stroed, first elememnt [0] is always name of file being executes, so first name is argv[1], and last name is argv[2]
# choose a name
  python3 emails.py Blossom Gill
# output: blossom@abc.edu
# create a new file in scripts directory to write test cases
  nano ~/scripts/emails_test.py
# use unittest package for 
# add shebang line and import packages
#!/usr/bin/env python3
import unittest
# import find_email() function which is defined in the script emails.py
from emails import find_email
# create class
class EmailsTest(unittest.TestCase):
# test case is created by subclassing unittest.TestCase, write first basic test case test_basic
  def test_basic(self):
    testcase = [None, "Bree", "Campbell"]
    expected = "breee@abc.edu"
    self.assertEqual(find_email(testcase), expected)
if __name__ == '__main__':
    unittest.main()
# test case has parameters to be passed to script emails.py
# script file = first element of input parameters through command line argv
# pass None in place of script file, call later in the script
# Adding to None, pass first name and last name as parameters
# variable stores expected value to be returned by emails.py
# assertEqual method passes test case to find_email() function imported earlier, checks if it generated expected output
# ctrl-o, Enter, ctrl-x to save
# run through command line, give permission to execute
chmod +x emails_test.py
# run
./emails_test.py
# output: Ran 1 test in 0.000s
# output: OK

# missing parameters
python3 emails.py Kirk
# output: **error message**
# write test case to handle this error
# test case should pass the first name to the script
# add test case test_one_name after first test case
# note down the name of the test cases, knowing the names will be helpful in running individual tests
nano emails_test.py
  def test_one_name(self):
    testcase = [None, "John"]
    expected = "Missing parameters"
    self.assertEqual(find_email(testcase), expected)
# updated script now looks like
#!/usr/bin/env python3

import unittest
from emails import find_email

class TestFile(unittest.TestCase):
  def test_basic(self):
    testcase = [None, "Bree", "Campbell"]
    expected = "breee@abc.edu"
    self.assertEqual(find_email(testcase), expected)
  def test_one_name(self):
    testcase = [None, "John"]
    expected = "Missing parameters"
    self.assertEqual(find_email(testcase), expected)
if __name__ == '__main__':
  unittest.main()
# ctrl-o, Enter, ctrl-x to Save
# run
./emails_test.py
# output: **error message**

# fix code
# solution 1: use a try/except clause to handle IndexError
# solution 2: check length of input parameters before travering the user_emails.csv file for email address
# test cases should pass, script should return "Missing parameters"

# try/except clause
# first execute the try clause
# if no exception occurs, the except clause is ignored
# if exception occurs duritn the execution of the try clause, the rest of try clause is then skipped
# attempts to match the type with the exception named after the except keyword
# if matches, the except clause is executed
# if not, the control is passed on to outer try statements
# if no handler is found, it's an unhandled exception, and execution stops with error message

# add try/except clause
nano emails.py
# add complete code block with find_email(argv) funtion which is within the try block
# add an IndexError execption within the except block
# execution will start normally with any number of parameters given to script
# if find_email(argv) function receives required number of parameters, it will return email address
# if not the required number of parameters, it will throw IndexError exception
# except clause would execute
def find_email(argv):
  # return email address based on username given
  # create username based on command line input
  try:
    fullname = str(argv[1] + " " + argv[2])
  # preprocess data
    email_dict = populate_dictionary('/home/<username>/data/user_emails.csv')
  # find and print email
    return email_dict.get(fullname.lower())
  except IndexError:
    return "Missing parameters"

# complete emails.py file now looks like
#!/usr/bin/env python3

import sys
import csv

def populate_dictionary(filename):
  # populate dictionary with name/email pairs for easy lookup
  email_dict = {}
  with open(filename) as csvfile:
    lines = csv.reader(csvfile, delimiter = ',')
    for row in lines:
      name = str(row[0].lower())
      email_dict[name] = row[1]
  return email_dict
def find_email(argv):
  # return email address based on username given
  # create username based on command line input
  try:
    fullname = str(argv[1] + " " + argv[2])
    # preprocess data
    email_dict = populate_dictionary('/home/<username>/data/user_emails.csv')
    # find and print email
    return email_dict.get(fullname.lower())
  except IndexError:
    return "Missing parameters"
  def main():
    print(find_email(sys.argv))
  if __name__ == "__main__":
    main()
# ctrl-o, Enter, ctrl-x to save
# run
./emails_test.py

# search for a non-existent employee
# expected output = "No email address found"
# add test case in emails_test.py file after second test case
# open emails_test.py
nano emails_test.py
# add following test case
  def test_two_name(self):
    testcase = [None, "Roy", "Cooper"]
    expected = "No email address found"
    self.assertEqual(find_email(testcase), expected)

# updated file
#!/usr/bin/env python3

import unittest
from emails import find_email

class TestFile(unittest.TestCase):
  def test_basic(self):
    testcase = [None, "Bree", "Campbell"]
    expected = "breee@abc.edu"
    self.assertEqual(find_email(testcase), expected)
  def test_one_name(self):
    testcase = [None, "John"]
    expected = "Missing parameters"
    self.assertEqual(find_email(testcase), expected)
  def test_two_name(self):
    testcase = [None, "Roy", "Cooper"]
    expected = "No email address found"
    self.assertEqual(find_email(testcase), expected)

if __name__ == '__main__':
  unittest.main()
# ctrl-o, Enter, ctrl-x to Save
# run
./emails_test.py
# failed
# use email_dict.get(full) method = fetches email address from list if found, None if not found
# add an if-else loop to return only if email_dict.get(username) returns valid email, "No email address found" if not
# edit script
nano emails.py
# located return email_dict.get(fullname.lower()) within script under find_email(argv) function and replace it with
#!/usr/bin/env python3

import sys
import csv

def populate_dictionary(filename):
  # populate dictionary with name/email pairs for easy lookup
  email_dict = {}
  with open(filename) as csvfile:
    lines = csv.reader(csvfile, delimiter = ',')
    for row in lines:
      name = str(row[0].lower())
      email_dict[name] = row[1]
  return email_dict
def find_email(argv):
  # return email address based on username given
  # create username based on command line input
  try:
    fullname = str(argv[1] + " " + argv[2])
    # preprocess data
    email_dict = populate_dictionary('/home/<username>/data/user_emails.csv')
    # find and print email
    # if email exists, print
    if email_dict.get(fullname.lower()):
      return email_dict.get(fullname.lower())
    else:
      return "No email address found"
  except IndexError:
    return "Missing parameters"
  def main():
    print(find_email(sys.argv))
  if __name__ == "__main__":
    main()
# ctrl-o, Enter, ctrl-x to save
# run
python3 emails_test.py
# output: Ran 3 tests i 0.001s
# output: OK
python3 emails.py Roy Cooper
# output: No email address found
