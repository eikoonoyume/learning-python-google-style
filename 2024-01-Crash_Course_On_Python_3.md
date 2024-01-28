# Instance Methods
# define Class Piglet with self parameter
class ClassName:
  def method_name(self, other_parameters):
    body_of_method
    
# Example    
class Piglet:
  def speak(self):
    print("oink oink")

# New Instance in Class Piglet
hamlet = Piglet()
hamlet.speak()
oink oink

# self.name
class Piglet:
  name = "piglet"
  def speak(self):
    print("Oink! I'm ()! Oink!".format(self.name))

hamlet = Piglet()
hamlet.name = "Hamlet"
hamlet.speak()
Oink! I'm Hamlet! Oink!

# 2nd Instance in Class Piglet
petunia = Piglet()
petunia.name = "Petunia"
petunia.speak()
Oink! I'm Petunia! Oink!

# Add Parameters
class Piglet:
  years = 0
  def pig_years(self):
    return self.years * 18

piggy = Piglet()
print(piggy.pig_years())
# output: 0
piggy.years = 2
print(piggy.pig_years())
# output: 36

# Example
class Apple:
  def __init__(self):
      self.color = "red"
      self.flavor = "sweet"
honeycrisp = Apple()
print(honeycrisp.color)
# output: "red"

# Modify Variables
class Apple:
  def __init__(self, color, flavor):
    self.color = color
    self.flavor = flavor
  honeycrisp = Apple("red", "sweet")
  fuji = Apple("red", "tart")
  print(honeycrisp.color)
  # output: "red"
  print(fuji.flavor)
  # output: "tart"

# __init__ double underscore = dunder method, init = constructor, first constructor must be self
# __str__() = converts objects to string
class Apple:
  def __init__(self, color, flavor):
    self.color = color
    self.flavor = flavor
  def __str__(self):
    return "an apple which is {} and {}".format(self.color, self.flavor)

  honeycrisp = Apple("red", "sweet")
  print(honeycrisp)
  # output: "an apple which is red and sweet"

# __len__ returns object/collection length
# __contains__ tests whether object contains an item
# __eq__ tests whether objects are equal
# __nq__ not equal
# __lt__ less than
# __gt__ greater than
# __le__ less or equal
# __ge__ greater or equal
# __repr__ define unambiguous string representations

# Example
class Apple:
  def __init__(self, color, flavor):
    self.color = color
    self.flavor = flavor
  def __str__(self):
    return "This apple is {} and its flavor is {}".format(self.color, self.flavor)
jonagold = Apple("red", "sweet")
print(jonagold)
# output: This apple is red and its flavor is sweet

# Example
class Triangle:
  def __init__(self, base, height):
    self.base = base
    self.height= height
  def area(self):
    return self.area() + other.area()
triangle1 = Triangle(10, 5)
triangle2 = Triangle(6, 8)
print("The area of triangle 1 is", triangle1.area())
# output: The area of triangle 1 is 25.0
print("The area of triangle 2 is", triangle2.area())
# output: The area of triangle 2 is 24.0
print("The area of both triangles is", triangle1 + triangle2)
# output: The area of both triangles is 49.0

# More Efficient Version of that Example
class Triangle:
  def __init__(self, base, height):
    self.base = base
    self.height = height
  def area(self):
    return 0.5 * self.base * self.height
  def __add__(self, other):
    return self.area() + other.area()
triangle1 = Triangle (10, 5)
triangle2 = Triangle(6, 8)
print("The area of triangle 1 is", triangle1.area())
# output: The area of triangle 1 is 25.0
print("The area of triangle 2 is", triangle2.area())
# output: The area of triangle 2 is 24.0
print("The area of both triangles is", triangle1 + triangle2)
# output: The area of both triangles is 49.0

# Using docstrings to show example of using the function and expected output
class ClassName:
  """Documentation for the class."""
  def method_name(self, other_parameters):
    """Documentation for the method."""
    body_of_method
  def function_name(parameters):
    """Documentation for the function."""
    body_of_function
  def my_function(x):
    """
    Sample usage:
    >>> my_function("example input")
    "example output"
    """
# Using help(some_function) if Python section in interactive to display docstrings
help(some_function)

# or
some_function.__doc__
print(some_function.__doc__)
function_explanation = other_function.__doc__

# Example
def task_reminder(time_as_string):
  if time_as_string == "8:00 a.m.":
    task = "Check overnight backup images"
  elif time_as_string == "11:30 a.m.":
    task = "Run TPS report"
  elif time_as_string == "5:30 p.m.":
    task = "Reboot servers"
  else:
    task = "Provide IT Support to employees"
  return task
print(task_reminder("10:00 a.m."))
# output: Provide IT Support to employees

# Example
def product (a, b):
  return(a*b)
print(product(product(2,4), product(3,5)))

# Example
def difference(a, b):
  return(a-b)
def sum(a, b):
  return(a+b)
print(difference(sum(2,2), sum(3,3)))

# Example
print((5 >= 2*4) and (5 <= 4*3))

# Example
x = 3
if x+5 > x**2 or x % 4 != 0:
  print("This comparison is True")

# Example
number = 6
if number * 2 < 14:
  print(number * 6 % 3)
elif number > 7:
  print(100 / number)
else:
  print(7 - number)

# Example
def get_remainder(x, y):
  if x == 0 or y == 0 or x == y:
    remainder = 0
  else:
    remainder = (x % y) / y
  return remainder
print(get_remainder(10, 3)

# Example
def convert_distance(km):
  m = km * 1000
  return m
my_trip_kilometers = 55
my_trip_meters = convert_distance(my_trip_kilometers)
print("The distance in meters is " + str(my_trip_meters))
# output
# The distance in meters is 55000

# Example
def convert_distance(miles):
  km = miles * 1.6
  return km
my_trip_meters = 55
my_trip_km = convert_distance(my_trip_miles)
print("The round-trip in kilometers is " + str(my_trip_km*2)) # double the trip to calculate round trip

# Example
def greeting(name):
  if name == "Taylor":
    return "Welcome back Taylor!"
  else:
    return "Hello there, " + name
print(greeting("Taylor"))
print(greeting("John"))
# output
# Welcome back Taylor!
# Hello there, John

# Example
if number > 11:
  print(0)
elif number != 10:
  print(1)
elif number >= 20 or number <12:
  print(2)
else:
  print(3)

# Example
name = "Marjery"
home_address = "1234 Mockingbird Lane"
print(name + " lived at her home address of " + home_address
# output
# Marjery lives at her home address of 1234 Mockingbird Lane

# Example
# Students in a class receive their grades as Pass/Fail. Scores of 60 or more (out of 100) mean that the grade is "Pass". For lower scores, the grade is "Fail". In addition, scores above 95 (not included) are graded as "Top Score". 
def exam_grade(score):
  if score > 95:
    grade = "Top Score"
  elif score >=60:
    grade = "Pass"
  else:
    grade = "Fail"
  return grade
print(exam_grade(65))
print(exam_grade(55))
print(exam_grade(60))
print(exam_grade(95))
print(exam_grade(100))
print(exam_grade(0))
# output
# Pass
# Fail
# Pass
# Pass
# Top Score
# Fail

# Example
def complementary_color(color):
  if color == "blue":
    complement = "orange"
  elif color == "yellow":
    complement = "purple"
  elif color == "red":
    complement = "green"
  else:
    complement = "unknown"
  return complement

print(complementary_color("blue"))
print(complementary_color("yellow"))
print(complementary_color("red"))
print(complementary_color("black"))
print(complementary_color("Blue"))
print(complementary_color(""))
# output
# orange
# purple
# green
# unknown
# unknown
# unknown


# Example
# The fractional_part function divides the numerator by the denominator, and returns just the fractional part (a number between 0 and 1). Complete the body of the function so that it returns the right number. Note: Since division by 0 produces an error, if the denominator is 0, the function should return 0 instead of attempting the division.
def fractional_part(numerator, denominator):
  if denominator == 0 or numerator == 0 or numerator % denominator == 0:
    part = 0
  else:
    part = (numerator%denominator)/denominator
  return part

print(fractional_part(5, 5))
print(fractional_part(5, 4))
print(fractional_part(5, 3))
print(fractional_part(5, 2))
print(fractional_part(5, 0))
print(fractional_part(0, 5))
# output
# 0
# 0.25
# 0.66...
# 0.5
# 0
# 0

# Example
multiplier = 1
result = multiplier * 5
while result <= 50:
  if result > 50:
    break
  print(result)
  multiplier += 1
  result = multiplier * 5
print("Done")
# output
# 5
# 10
# 15
# 20
# 25
# 30
# 35
# 40
# 45
# 50
# Done

# Example
def count_factors(given_number):
  factor = 1
  count = 1
  if given_number == 0:
    return 0
  while factor < given_number:
    if given_number % factor == 0:
      count += 1
    factor += 1
  return count
print(count_factors(0))
print(count_factors(3))
print(count_factors(10))
print(count_factors(24))
# output
# 0
# 2
# 4
# 8

# Example
def addition_table(given_number):
  iterated_number = 1
  my_sum = 1
  while iterated_number <= 5:
    my_sum = given_number + iterated_number
    if my_sum > 20:
      break
  print(str(given_number), "+", str(iterated_number), "=", str(my_sum))
  iterated_number += 1
addition_table(5)
addition_table(17)
addition_table(30)

# output
# 5 + 1 = 6
# 5 + 2 = 7
# 5 + 3 = 8
# 5 + 4 = 9
# 5 + 5 = 10
# 17 + 1 = 18
# 17 + 2 = 19
# 17 + 3 = 20

# Example
for number in range(1, 6+1, 2):
  print(number * 3)
# output
# 3
# 9
# 15

for number in range(2,8):
  print(number*2)
# output
# 4
# 9
# 16
# 25
# 36
# 49

# Example
for x in range(2):
  print("This is the outer loop iteration number " + str(x))
  for y in range(3+1):
    print("Inner loop iteration number " + str(y))
  print("Exit inner loop")
# output
# This is the outer loop iteration number 0
# Inner loop iteration number 0
# Inner loop iteration number 1
# Inner loop iteration number 2
# Inner loop iteration number 3
# Exit inner loop
# This is the outer loop iteration number 1
# Inner loop iteration number 0
# Inner loop iteration number 1
# Inner loop iteration number 2
# Inner loop iteration number 3
# Exit inner loop

# Example
for x in range(7):
  if x % 2 == 0:
    print(x)
# output
# 0
# 2
# 4
# 6

# Example of list comprehension version
even_numbers = [x for x in range(7) if x % 2 == 0]
print(even_numbers)
# output
# [0, 2, 4, 6]

# Example of list comprehenion
sequence = range(10)
new_list = [x for x in sequence if x % 2 == 0]
# output
# [0, 2, 4, 6, 8]

# Example
def count_by_10(end):
  count = ""
  for number in range(0,end+1,10):
    count += str(number) + " "
  return count.strip()
print(count_by_10(100))
# output
# 0 10 20 30 40 50 60 70 80 90 100

# Example
def matrix(initial_number, end_of_first_row):
  n1 = initial number # making shorter names for variables
  n2 = end_of_first_row+1 # need to include the upper range
  for column in range(n1, n2):
    for row in range(n1, n2):
      print(column*row, end=" ")
  print()
  matrix(1,4)
  # output
  # 1 2 3 4 
  # 2 4 6 8 
  # 3 6 9 12 
  # 4 8 12 16 

# Example
for outer_loop in range(10):
  for inner_loop in range(outer_loop):
    print(inner_loop)
# output
# 0
# 0
# 1
# 0
# 1
# 2
# 0
# 1
# 2
# 3
# 0
# 1
# 2
# 3
# 4
# 0
# 1
# 2
# 3
# 4
# 5
# 0
# 1
# 2
# 3
# 4
# 5
# 6
# 0
# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 0
# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8

# Example
for n in range(11,-2):
  if n % 2 != 0:
    print(n, end= " ")
# output
# 11, 9, 7, 5, 3, 1

# Example
starting_number = 18
while starting_number >= 0:
  print(starting_number, end=" ")
  starting_number -= 3 # decrement by 3
# output
# 18 15 12 9 6 3 0

# Example
def X_figure(salary):
  tally = 0
  if salary == 0:
    tally += 1
  while salary >=1:
    salary = salary/10
    tally += 1
  return tally
print("The CEO has a " + str(X_figure(2300000)) + "-figure salary.")
# output
# The CEO has a 7-figure salary.

# Example
def elevator_floor(enter, exit):
  floor = enter
    elevator_direction = ""
    if enter > exit:
      elevator_direction = "Going down: "
    while floor >= exit:
      elevator_direction += str(floor)
      if floor > exit:
        elevator_direction += " | " # the pipe symbol
      floor -= 1
    else:
      elevator_direction = "Going up: "
      while floor <= exit:
        elevator_direction += str(floor)
        if floor < exit:
          elevator_direction += " | "
        floor += 1
    return elevator_direction
  print(elevator_floor(1,4))
  print(elevator_floor(6,2))
  # output
  # Going up: 1 | 2 | 3 | 4
  # Going down: 6 | 5 | 4 | 3 | 2

  # Example
  # Fill in the blanks to print the numbers from 15 to 5, counting down by fives.
  number = 15
  while number >= 5:
    print(number, end=" ")
    number -= 5
  # output
  # 15 10 5

  # Example
  # Find and correct the error in the for loop.  The loop should print every number from 5 to 0 in descending order.
  for number in range(5,-1,-1):
    print(number)
  # output
  # 5
  # 4
  # 3
  # 2
  # 1
  # 0

  # Example
  # Fill in the blanks to complete the function “digits(n)” to count how many digits the given number has. For example: 25 has 2 digits and 144 has 3 digits.
Tip: you can count the digits of a number by dividing it by 10 once per digit until there are no digits left.
  def digits(n):
    count=0
    if n == 0:
      count += 1
    while (n>0):
      count += 1
      n = n//10
    return count
  print(digits(25))
  print(digits(144))
  print(digits(1000))
  print(digits(0))
  # output
  # 2
  # 3
  # 4
  # 1

  # Example
  # Fill in the blanks to complete the “multiplication_table” function. This function should print a multiplication table, where each number is the result of multiplying the first number of its row by the number at the top of its column. 
  def multiplication_table(start, stop):
    for x in range(start, stop+1):
      for y in range(start, stop+1):
        print(str(x*y), end=" ")
      print()
  multiplication_table(1,3)
  # output
  # 1 2 3
  # 2 4 6
  # 3 6 9

  # Example
  # Fill in the blanks to complete the “divisible” function. This function should count the number of values from 0 to the “max” parameter that are evenly divisible (no remainder) by the “divisor” parameter. Complete the code so that a function call like “divisible(100,10)” will return the number “10”.
  def divisible(max, divisor):
    count = 0
    for x in range(0, max):
      if x % divisor == 0:
        count += 1
    return count
  print(divisible(100, 10))
  print(divisible(10, 3))
  print(divisible(144, 17))
  # output
  # 10
  # 4
  # 9

  # Example
  # Fill in the blanks to complete the “odd_numbers” function. This function should return a space-separated string of all odd positive numbers, up to and including the “maximum” variable that's passed into the function. Complete the for loop so that a function call like “odd_numbers(6)” will return the numbers “1 3 5”.
  def odd_number(maximum):
    return_string = ""
    for x in range(0, maximum+1):
      if x % 2 != 0:
        return_string += str(x) + " "
    return return_string.strip()
  print(odd_numbers(6))
  print(odd_numbers(10))
  print(odd_numbers(1))
  print(odd_numbers(3))
  print(odd_numbers(0))
  # output
  # 1 3 5
  # 1 3 5 7 9
  # 1
  # 1 3
  # no numbers will be displayed for this one

  # Example
  # The following code causes an infinite loop. Can you figure out what’s incorrect and how to fix it?
  def count_to_ten():
    x = 1
    while x <= 10:
      print(x)
      x = x + 1
  count_to_ten()
  # output
  # 1
  # 2
  # 3 
  # 4
  # 5
  # 6
  # 7
  # 8 
  # 9
  # 10

  # Example
  # Fill in the blanks to print the even numbers from 2 to 12.
  number = 2
  while number <= 12:
    print(number, end=" ")
    number += 2
  # output
  # 2 4 6 8 10 12

  # Example
  # Fill in the blanks to complete the “factorial” function. This function will accept an integer variable “n” through the function parameters and produce the factorials of this number (by multiplying this value by every number less than the original number [n*(n-1)], excluding 0).  
  def factorial(n):
    result = n
    start = n
    n -= 1
    while n>0:
      result *= n # multiplies current result by current value of n
      n -= 1
    return result
  print(factorial(3))
  print(factorial(9))
  print(factorial(1))
  # output
  # 6
  # 362880
  # 1

  # Example
  # Fill in the blanks to complete the “sequence” function. This function should print a sequence of numbers in descending order, from the given "high" variable to the given "low" variable.  The range should make the loop run two times.
  def sequence(low, high):
    for x in range(high, low-1, -1): # this part is probably incorrect, this part needs to make the loop run twice
      for y in range(high, low-1, -1):
        if y == low:
          print(str(y))
        else:
          print(str(y), end= ", ")
    sequence(1, 3)
  # output
  # 3, 2, 1 (it comes out 3 times instead of twice so it's incorrect)

  # Example
  # Fill in the blanks to complete the “counter” function. This function should count down from the “start” to “stop” variables when “start” is bigger than “stop”. Otherwise, it should count up from “start” to “stop”. Complete the code so that a function call like “counter(3, 1)” will return “Counting down: 3, 2, 1” and “counter(2, 5)” will return “Counting up: 2, 3, 4, 5”.
    def counter(start, stop):
      if start > stop:
        return_string = "Counting down: "
        while start >= stop: # to include the last stop number
          return_string += str(start)
            if start > stop:
              return_string += ","
            start -= 1
        else:
          return_string = "Counting up: "
          while start <= stop:
            return_string += str(start)
            if start < stop:
              return_string += ","
            start += 1
          return return_string
  print(counter(1, 10))
  print(counter(2, 1))
  print(counter(5, 5))
  # output
  # Counting up: 1,2,3,4,5,6,7,8,9,10
  # Counting down: 2,1
  # Counting up: 5

  # Example
  # Fill in the blanks to complete the “all_numbers” function. This function should return a space-separated string of all numbers, from the starting   “minimum” variable  up to and including the “maximum” variable that's passed into the function. Complete the for loop so that a function call like “all_numbers(3,6)” will return the numbers “3 4 5 6”.  
  def all_numbers(minimum, maximum):
    return_string = "" # initializing
    for x in range(minimum, maximum+1):
      return_string += str(x) + " "
    return return_string.strip()
  print(all_numbers(2,6))
  print(all_numbers(3,10))
  print(all_numbers(-1,1))
  print(all_numbers(0,5))
  print(all_numbers(0,0))
  # output
  # 2 3 4 5 6
  # 3 4 5 6 7 8 9 10
  # -1 0 1
  # 0 1 2 3 4 5
  # 0
