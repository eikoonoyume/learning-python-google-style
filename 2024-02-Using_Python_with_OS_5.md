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


