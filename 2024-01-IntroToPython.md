# Intro to Python
print("Hello, World!")

# Print a greeting with a name
name = "Brook"
print("Hello, " + name)
# output: Hello, Brook

# Python Calculator
# Addition
print(4+5)
# output: 9

# Multiplication
print(9*7)
# output: 63

# Division
print(-1/4)
# output: -0.25

print (1/3)
# output: 0.33333333333

# Division and Subtraction
print(((2050/5)-32)/9)
# output: 42.0

# To the power of 10
print(2**10)
# output: 1024

# Arithmetic Operators
# x + y # addition
# x - y # subtraction
# x * y # multiplication
# x / y # division
# x**e # exponent "to the power of"
# x**2 # squared
# x**3 # cubed
# x**(1/2) # square root
# x // y # floor division, returns integer part of division
# x % y # modulo operator, returns remainder part of division

# Order of Operations PEMDAS
# (), {}, []
# exponents
# multiplication, division
# addition, subtraction

# Printing Text String
print("I love Python!")
# output: I love Python!

# Printing Numeric Values
print(360)
print(32*45)
# output: 360
# output: 1440

# Printing Value of a Variable
value = 8*6
print(value)
# output: 48

# Assigning Values to Variables, or Arithmetic Calculation to Variables
years = 10
weeks_in_a_year = 52
weeks_in_a_decade = years * weeks_in_a_year

print(weeks_in_a_decade)
# output: 520

# Return Data Types
print(type("a"))
print(type(2))
print(type(2.5))
# output: 'str'
# output: 'int'
# output: 'float'
# float = decimal number, integer = whole number

# Adding Strings
print("a"+"b"+"c")
# output: abc

print("This " + "is " + "pretty " + "neat!")
output: This is pretty neat!

# Changing Data Types + Calculating
# str()
# int()
# float()
base = 6
height = 3
area = (base*height)/2
print("The area of the triangle is:" + str(area))
# output: The area of the triangle is: 9.0

# Changing Data Types + Calculating
hotel_room = 100
tax = hotel_room * 0.08
total = hotel_room + tax
room_guests = 4
share_per_person = total/room_guests

print("Each person needs to pay: " + str(share_per_person))

# Forming Sentences and Creating Spaces
salutation = "Dr."
first_name = "Prisha"
middle_name = "Jai"
last_name = "Agarwal"
suffix = "Ph.D."

print(salutation + " " + first_name + " " + middle_name + " " + last_name + ", " + suffix)
# output: Dr. Prisha Jai Agarwal, Ph.D

# OR

print(salutation, first_name, middle_name, last_name, ",", suffix)
# output: Dr. Prisha Jai Agarwal , Ph.D

# Resolve TypeError from Data Type Mismatch
print("5 * 3 = "  + (5*3))

# Change to
print("5 * 3 = " + str(5*3))

# Resolve ZeroDivisionError from dividing by 0
numerator = 7
denominator = 0 # Change the denominator value
result = numerator / denominator
print(result)

# Resolution
numerator = 7
denominator = 1
result = numerator / denominator
print(result)
# output = 7

# Defining
def greeting(name, department):
  print("Welcome, " + name)
  print("You are part of " + department)
greeting("Blake", "IT Support")
greeting("Ellis", "Software Engineering")
# output: Welcome, Blake
# You are part of IT Support
# output: Welcome, Ellis
# You are part of Software Engineering

# Sorting in Ascending Order, no change in iterable
time_list = [12, 2, 32, 19, 57, 22, 14]
print(sorted(time_list))
# output: [2, 12, 14, 19, 22, 32, 57]

time_list = [12, 2, 32, 19, 57, 22, 14]
print(sorted(time_list))
print(time_list)
# output: [2, 12, 14, 19, 22, 32, 57]
# output: [12, 2, 32, 19, 57, 22, 14]

# Maximum and Minimum
time_list = [12, 2, 32, 19, 57, 22, 14]
print(min(time_list))
print(max(time_list))
# output: 2
# output: 57

# Using Built-in Function Sum
def area_triangle(base, height):
  return base*height/2
area_a = area_triangle(5,4)
area_b = area_triangle(7,3)

sum = area_a + area_b
print("The sum of both areas is: " + str(sum))
# output: The sum of both areas is: 20.5

# Calculation
def convert_seconds(seconds):
  hours = seconds // 3600 # floor division, returning integer part of division
  minutes = (seconds - hours * 3600) // 60
  remaining_seconds = seconds - hours * 3600 - minutes * 60
  return hours, minutes, remaining_seconds

hours, minutes, seconds = convert_seconds(5000)
print(hours, minutes, seconds)
# output: 1 23 20

# Defining
def greeting(name): # defining greeting as the name
  print("Welcome, " + name) 
result = greeting("Christine")
print(result)
# output: Welcome, Christine

# Length
name = "Kay"
number = len(name) * 0
print("Hello " + name + ". Your lucky number is " + str(number))

name = "Cameron"
number = len(name) * 9
print("Hello " + name + ". Your lucky number is " + str(number))
# output: Hello Kay. Your lucky number is 27
# output: Hello Cameron. Your lucky number is 63

# More Efficient Version of the Length Code
def lucky_number(name): # defining lucky numner as the name
  number = len(name) * 9  # the name is the length of the name times 9
  print("Hello " + name + ". Your lucky number is " + str(number))

lucky_number("Kay") # so say the lucky number is Kay, said in the same way as the definition
lucky_number("Cameron") # so say the lucky number is Cameron, said in the same way as the definition
# output: Hello Kay. Your lucky number is 27
# output: Hello Cameron. Your lucky number is 63

# Calculating 
def circle_area(radius): # defining the area of a circle as the radius
  pi = 3.14 # you'll need to know pi
  area = pi * (radius ** 2)  # you'll need to know are will be pi times radius squared
  print(area) # print the area as the result

circle_area(5) # so if circle_area is 5
# output: 78.5

# Using Return to send result
def find_total_days(years, months, days): # defining find total days as years, months days
  my_days = (years*365) + (months*30) + days
  return my_days # will send result of the my_days calculation

print(find_total_days(2,5,23)) # given 2 years, 5 months, 23 days
# output: 903

# Example
def print_seconds(hours, minutes, seconds):
  print(3600*hours+60*minutes+seconds)
print_seconds(1,2,3)
# output: 3723

# Calculating
def convert_volume(fluid_ounce): defining convert volume as fluid ounce
  ml = fluid_ounce * 29.5 # you need to know ml is fluid ounce multiplied by 29.5
  return ml # return the result of the calculation

print("The volume in millimeters is " + str(convert_volume(2)))
# output: The volume in millimeteres is 59.0

print("The volume in millimeters is " + str(convert_volume(2)*2)) # doubling the fluid ounces
# output: The volume in millimeters is 118.0

# or you could just
print("The volume in millimeters is " + str(convert_volumme(4))
# output: The volume in millimeters is 118.0
