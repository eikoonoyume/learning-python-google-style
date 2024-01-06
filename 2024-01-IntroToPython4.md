# While Loops
x = 0
while x < 5:
  print("Not there yet, x=" + str(x)) # after printing
  x = x + 1 # goes back to x, but adds a 1, loops again
print("x=" + str(x))
# output:
# Not there yet, x=0
# Not there yet, x=1
# Not there yet, x=2
# Not there yet, x=3
# Not there yet, x=4

# Example
x = 0
while x < 5:
  print("Not there yet, x=" + str(x))
  x = x + 1
print("x=" + str(x))
x=5

# Example
def attempts(n):
  x = 1
  while x <= n:
    print("Attempt " + str(x)) # after printing
    x += 1 # goes back to x, keeps adding 1 each loop
  print("Done")
attempts(5)
# output:
# Attempt 1
# Attempt 2
# Attempt 3
# Attempt 4
# Attempt 5
# Done

# Example
username = get_username()
while not valid_username(username):
  print("Invalid username")
  username = get_username()

  # the loop will continue to iterate as long as the value of the username variable is not valid
  # the username variable is assigned the value returned by get_username() function like it did at the beginning

# Error Example
while my_variable < 10:
  print("Hello")
  my_variable += 1

  # variable wasn't assigned/defined so this will become an error

 # Example
 my_variable = 5
 while my_variable < 10:
   print("Hello")
   my_variable += 1
  # output:
  # Hello
  # Hello
  # Hello
  # Hello
  # Hello

  # Example
  x = 1
  sum = 0
  while x < 10:
    sum = sum + x
    x = x + 1
  product = 1
  while x < 10:
    product = product * x
    x = x + 1
  print(sum, product)
  # output: 45 1

  # Error Example
  def count_down(start_number):
    while (current > 0):
      print(current
      current -= 1
    print("Zero!")
    # output: Error because it was start_number but was changed to "current"

  # Example
  def count_down(start_number):
    while (start_number > 0):
    print(start_number)
    start_number -= 1
  print("Zero!")

  count_down(3)
  # output:
  # 3
  # 2 
  # 1
  # Zero!

  # Iterative Statement Examples
  # Printing 3 numbers from a list
  for i in [8, 9, 10]:
    print(i)
  # Printing the message "Access denied" 5 times
  for i in range(5):
    print("Access denied")
  # Printing sequence of numbers starting at 10, ending at 30
  for i in range(10, 31):
    print(i)
  # Printing a message that tells the user to "try again" as long as the value of the attempt variable is 5 or less
  # You want the value of the variable to increase by 1 each time it passes through the loop
  attempt = 1
  while (attempt <= 5):
    print("Try again.")
    attempt = atempt + 1
  # Printing the numbers 20, 19, 18, 17, and 16.
  i = 20
  while (i > 15):
    print(i)
    i = i - 1
  # Welcome 3 users from a list by their name (for example, "Welcome, Emerick Larson.")
  name = ["Emerick Larson", "Estrella Ortiz", "Tori Shah"]
  for i in name:
    print("Welcome,", i)

  # Breaking Loops
  if x != 0: # if x is not 0
    while x % 2 == 0:
      x = x / 2

  # Or
  while x != 0 and x % 2 == 0:
    x = x / 2
  # only entering loop for values of x that are both different than 0 and even

  # Infinite Loop
  def print_range(start, end): # loops through the numbers from start to end
    n = start
    while n <= end:
      print(n)
  print_range(1, 5) 
  # output: 
  # 1
  # 2
  # 3
  # 4 
  # 5

  # Breaking the Loop
  def print_range(start, end):
    n = start
    while n <= end:
      print(n)
      n += 1
  print_range(1, 5)
  # output
  # 1
  # 2
  # 3 
  # 4
  # 5

  # Break
  while True:
    do_something_cool()
    if user_requested_to_stop():
      break
  # Infinite Loop Example
  def is_power_of_two(number): # loop checks if number can be divided by two without remainder
    while number % 2 == 0:
      number = number / 2 # if after dividing by 2 and the number equals 1, then the number is a power of 2
    if number == 1:
      return True
    return False
  print(is_power_of_two(0))
  # output: False
  print(is_power_of_two(1))
  # output: True
  print(is_power_of_two(8))
  # output: True
  print(is_power_of_two(9))
  # output: False

  # Same Example, Avoiding ZeroDivisionError
  def is_power_of_two(number):
    if number == 0:
      return False
    while number % 2 == 0:
      number = number / 2
    if number == 1:
      return True
    return False
  print(is_power_of_two(0))
  # output: False
  print(is_power_of_two(1))
  # output: True
  print(is_power_of_two(8))
  # output: True
  print(is_power_of_two(9))
  # output: False

  # Example: Write a function that takes an argument n and returns the sum of integers from 1 to n. 
  # For example, if n=5, your function should add up 1+2+3+4+5 and return 15
  # If n is less than 1, just return 0. Use a whle loop to calculate this sum.
  def sum_of_integers(n):
    if n < 1:
      return 0
    i = 1
    sum = 
    while :
      sum = sum + i
      i = 
    return sum
  print(sum_of_integers(3))
  # output: 6
  print(sum_of_integers(4))
  # output: 10
  print(sum_of_integers(5))
  # output: 15

  # My Guess
  def sum_of_integers(n):
    if n < 1:
      return 0
    i = 1
    sum = 0 # or sum = n + (n-1, 0) or (n-=1)
    while n >= 1:
      sum = sum + i
      i = i + 1

  # Example
  def multiplication_table(number):
    multiplier = 1
    while multiplier <= 5
      result = number * multiplier
      if result > 25:
        break
      print(str(number) + "x" + str(multiplier) + "=" + str(result))
      multiplier += 1

  multiplication_table(3)
  # output:
  # 3x1=3
  # 3x2=6
  # 3x3=9
  # 3x4=12
  # 3x5=15

  multiplication_table(5)
  # output:
  # 5x1=5
  # 5x2=10
  # 5x3=15
  # 5x4=20
  # 5x5=25

  multiplication_table(8)
  # output:
  # 8x1=8
  # 8x2=16
  # 8x3=24
  
    
