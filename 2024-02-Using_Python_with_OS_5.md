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
