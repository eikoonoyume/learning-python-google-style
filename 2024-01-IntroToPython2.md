# Booleans: T or F
print(10>1)
# output: True

# == Both sides are equal
print("cat" == "dog")
# output: False

# != Both sides are NOT equal
print(1 != 2)
# output: True

# Integer and String Comparisons are Errors
print(1 < "1")
# output: Error

print(1 == "1")
# output: False

# String Comparisons: < > alphabetical order
print("Yellow" > "Cyan" and "Brown" > "Magenta")
# output: False

# Or Operator: either is true = True, both are false = False
print(25 > 50 or 1 != 2)
# output: True

# Not Operator: invert the answer, true becomes False, false becomes True
print(not 42 == "Answer")
# output: True

# > greater than, < less than, >= greater than or equal to, <= less than or equal to
print(32 == 30+2)
# output: True

print(5+10 == 6+7)
# output: False

print(10-4 != 10+4)
# output: True

print(9/3 != 3*1)
# output: False

# = Assignment Operator for Assigning Values vs == Equal
my_variable = 3*5
print(my_variable)
# output: 15

print(my_variable == 3*4)
# output: True

print(11 > 3*3)
# output: True

print(4/2 > 8-4)
# output: False

print(4/2 < 8-4)
# output: True

print(11 < 3*3)
# output: False

print(12*2 >= 24)
# output: True

print(18/2 >= 15)
# output: False

print(12*2 <= 30)
# output: True

print(15 <= 18/2)
# output: False

# Comparing Strings == and != to Check if it's the same text or not
print("a string" == "a string")
# output: True

print("4+5" == 4 + 5)
# output: False # because an integer and a string cannot be equal

print("rabbit" != "frog"
# output: True

event_city = "Shanghai"
print(event_city != "Shanghai")
# output: False

print("three" == 3)
# output: False
# Comparing Strings < > <= >= to alphabetize using Unicode / ASCII values
# Uppercase A-Z = 65 to 90
# lowercase a-z = 97 to 122
# Compare to see which has larger/smaller Unicode value

print("Wednesday" > "Friday")
# output: True

print("Brown" < "brown")
# output: True

print("sunbathe" > "suntan")
# output: False

print("Five" < 6)
# output: Error

var1 = "my computer" >= "my chair"
var2 = "Spring" <= "Winter"
var3 = "pineapple" >= "pineapple"
print("Is \"my computer\" greater than or equal to \"my chair\"? Result: ", var1)
print("Is \"Spring\" less than or equal to \"Winter\"? Result: ", var2)
print("Is \"pineapple\" less than or equal to \"pineapple\"? Result: ", var3)

# Logical Operators And Or Not
# and = both sides must be True to be True
# or = if one side is True, it's all True
# not = inverts the result, True becomes False, False becomes True

print((6*3 >= 18) and (9+9 <= 36/2))
# output: True

print("Nairobi" < "Milan" and "Nairobi" > "Hanoi")
# output: False

print(15/3 < 2+4) or (0 > 6-7))
# output: True

print(country == "New York City" or city == "New York City")
# output: True

print("B_name" > "C_name" or "B_name" < "A_name)
# output: False

x = 2*3 > 6
print("The value of x is:")
# output: The value of x is:
print(x)
# output: False
print("")
# output: *a space will be printed*
print("The inverse value of x is:")
# output The inverse value of x is:
print(not x)
# output: True

today = "Monday"
print(not today == "Tuesday")
# output: True

# if Statements
def hint_username(username): # defining hint_username as username
  if len(username) <3: # if the length of the username is less than 3
    print("Invalid username. Must be at least 3 characters long")

# else Statements
def hint_username(username):
  if len(username) < 3:
    print("Invalid username. Must be at least 3 characters long")
  else:
    print("Valid username") 
    # if the length is more than 3, then it'll be valid

# If Statement Example
def is_even(number):
  if number % 2 == 0: # if the remainder is equal to 0
    return True
  return False # otherwise return False, the "else" statement is optional

# Else Statement Example
def is_positive(number):
  if number > 0:
    return True
  else:
    return False
print(is_positive(-5))
# output: False
print(is_positive(0))
# output: False
print(is_positive(13))
# output: True

# Elif Statements
def hint_username(username):
  if len(username) < 3:
    print("Invalid username. Must be at least 3 characters long")
  elif:
    if len(username) > 15:
      print("Invalid username. Must be at most 15 characters long")
    else:
      print("Valid username")

# Examples
print(10*4 > 14+23)
# output: True

print("tall" < "short")
# output: False

def translate_error_code(error_code):
  if error_code == "401 Unauthorized":
    translation = "Server received an unautheticated request" # return value statement
  elif error_code == "404 Not Found"
    translation = "Requested web page not found on server"
  elif error_code == "408 Request Timeout":
    translation = "Server reqest to close unused connection"
  else:
    translation = "Unknown error code"
  return translation
print(translate_error_code("404 Not Found"))
# output: Requested web page not found on server

number = 25
if number <= 5:
  print("The number is 5 or smaller.")
elif number == 33:
  print("The number is 33.")
elif number < 32 and number >= 6:
  print("The number is less than 32 and greater than 6.")
else:
  print("The number is " + str(number))
# output: The number is less than 32 and greater than 6.

def round_up(number):
  x = 10
  whole_number = number // x
  remainder = number % x
  if remainder >= 5:
    return x*(whole_number+1)
  return x*whole_number
print(round_up(35))
# output: 40
