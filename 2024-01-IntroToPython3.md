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

