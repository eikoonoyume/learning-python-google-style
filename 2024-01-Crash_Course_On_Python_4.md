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
    sum = 0 
    while i <= n:
      sum = sum + i
      i = i + 1
    return sum
  print(sum_of_integers(3))
  # output: 6
  print(sum_of_integers(4))
  # output: 10
  print(sum_of_integers(5))
  # output: 15

  # The variable sum should be an accumulator, so when you are in the initial state, it should not have any value. It should be 0.
  # When you look at the while condition, n is a constant value.
  # Thus, n >= 1 is always true. The loop will never finish.
  # The while condition must take into consideration that the variable can be altered, the case in question being
  # the variable i that was initialized as 1
  # i > 1 won't work because all numbers more than 1 just makes an infinite loop
  # i >= 1 results in the same infinite loop
  # i > n, the n being all the numbers needed to sum up the number (meaning if it was 5, it stands for 1+2+3+4+5 all of these numbers), is the condition that is FALSE. I need the true statement.
  # Thus, n and anything less than n. i <= n

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
  
  # For Loops
  for x in range(5):
    print(x)
  # output:
  # 0
  # 1
  # 2
  # 3
  # 4

  # Example
  friends = ['Taylor', 'Alex', 'Pat', 'Eli']
    for friend in friends:
      print("Hi " + friend)
      
  # Example
  values = [23, 52, 59, 37, 48]
  sum = 0 # must start at 0
  length = 0 # must start at 0
  for value in values:
    sum + value
    length += 1 # must increment to keep the loop going
  print("Total sum: " + str(sum) + " - Average: " + str(sum/length))

  # Example
  product = 1
  for n in range(1,10):
    product = product * n
  print(product)
  # must start with 1 and not 0, if it was zero, the multiplication will result in 0

  # Example
  def to_celcius(x):
    return (x-32)*5/9
  for x in range(0,101,10):
    print(x, to_celcius(x))
  # output:
  # 0 -17.77777777
  # 10 -12.22222
  # 20 -6.6666666
  # etc

  # Example
  for n in range(1, 5, 6):
    print(n)
  # output:
  # 1

  # Example
  for n in range(0,11,2):
    print(n)
  # output:
  # 0
  # 2
  # 4
  # 6
  # 8
  # 10

  # Example
  for number in range(2,7+1) # add 1 to keep the 7 in the range
    print(number*3)
  # output:
  # 6
  # 9
  # 12
  # 15
  # 18
  # 21

  # Example
  for x in range(2, -2, -1): # -1 will decrement backwards
    print(x)
  # output:
  # 2
  # 1
  # 0
  # -1

  # Nested for loops
  for left in range(7):
    for right in range(left, 7):
      print("[" + str(left) + "| *the pipe symbol*" + str(right) + "]", end=" ") # keep this in mind when you need to add spaces
    print()
  # output:
  # [0|0] [0|1] [0|2] [0|3] [0|4] [0|5] [0|6]
  # [1|1] [1|2] [1|3] [1|4] [1|5] [1|6]
  # [2|2] [2|3] [2|4] [2|5] [2|6]
  # [3|3] [3|4] [3|5] [3|6]
  # [4|4] [4|5] [4|6]
  # [5|5] [5|6]
  # [6|6]

  # Example
  teams = [ 'Dragons', 'Wolves', 'Pandas', 'Unicorns']
  for home_team in teams:
    for away_team in teams:
      if home_team != away_team:
        print(home_team + " vs " + away_team)

  # output:
  # Dragons vs Wolves
  # Dragons vs Pandas
  # Dragons vs Unicorns
  # Wolves vs Dragons
  # Wolves vs Pandas
  # Wolves vs Unicorns
  # Pandas vs Dragons
  # Pandas vs Wolves
  # Pandas vs Unicorns
  # Unicorns vs Dragons
  # Unicorns vs Wolves
  # Unicorns vs Pandas

  # Strings and Loops
  greeting = "Hello"
  for c in greeting:
    print("The next character is: ", c)
  # output:
  # The next character is: H 
  # The next character is: e 
  # The next character is: l 
  # The next character is: l 
  # The next character is: o

  # Example
  for i in range(0, 3):
    print("The next value is:" i)
  # output:
  # The next value is 0 
  # The next value is 1 
  # The next value is 2


  # Loop Examples
  greeting = 'Hello'
  for char in greeting:
    print(char)
  # output:
  # H
  # e 
  # l 
  # l 
  # o

  # Example
  for i in range(len(greeting)):
    print(i)
  # output:
  # 0
  # 1
  # 2
  # 3
  # 4

  # Example
  greeting = 'Hello'
  index = 0
  while index < len(greeting):
    print(greeting[index])
    index += 1
  # output:
  # H
  # e
  # l 
  # l 
  # o

  # Example
  numbers = [1, 2, 3, 4, 5]
  squared_numbers = [x ** 2 for x in numbers]
  print(squared_numbers)
  # output: [1, 4, 9, 16, 25]

  # Example
  string1 = "Greetings, Earthlings"
  print(string1[0])
  print(string1[4:8])
  print(string1[11:])
  print(string1[:5})
  print(string1[-10:])
  print(string1[55:])
  print(string1[0::2})
  print(string1[::-1])
  # output:
  # G
  # ting
  # Earthlings
  # Greet
  # Earthlings
  # *doesn't print because placement 55 is empty space*
  # Getns atlns
  # sgnilhtraE ,sgniteerG
  
  # Example
  print("Hello" + " " + "world")
  # output: Hello world

  # Example
  greeting = ["Hello", "world"]
  print(" ".join(greetings))
  # output: Hello world

  # Example
  name = "Alice"
  print("Hello, " + name + "!")
  # output: Hello, Alice!

  # Example
  def format_phone(phonenum):
    area_code = "(" + phonenum[:3] + ")"
    exchange = phonenum[3:6]
    line = phonenum[-4:]
    return area_code + " " + exchange + "-" + line
  print(format_phone("2025551212"))
  # output: (202) 555-1212

  # Common Errors in Loops
  for x in 25:
    print(x)
  # this is a single element, and for loops are for multiple elements
  # can change by writing ' for x in range(25): '

  # Example
  def greet_friends(friends):
    for friend in friends:
      print("Hi " + friend)
  greet_friends(['Taylor', 'Luisa', 'Jamaal', 'Eli'])
  # output:
  # Hi Taylor
  # Hi Luisa
  # Hi Jamaal
  # Hi Eli

  # Version 2
  def greet_friends(friends):
    for friend in friends:
      print("Hi " + friend)
  greet_friends("Barry") # a list was not defined, need a list even if with only one element
  # output:
  # Hi B
  # Hi a
  # Hi r
  # Hi r
  # Hi y

  # Recursion
  def factorial(n):
    if n<2: # conditional block
      return 1
    return n*factorial(n-1) # recursive case for what doesn't apply to condition

    # Example
    def factorial(n):
      print("Factorial called with " + str(n))
      if n<2:
        print("Returning 1")
        return 1
      result = n*factorial(n-1)
      print("Returning " + str(result) + " for factorial of " + str(n))
      return result

      factorial(4)
      # output:
      # Factorial called with 4
      # Factorial called with 3
      # Factorial called with 2
      # Factorial called with 1
      # Returning 1
      # Returning 2 for factorial of 2
      # Returning 6 for factorial of 3
      # Returning 24 for factorial of 4
      # 24
      
  # Example
  # The function sum_positive_numbers should return the sum of all positive numbers between the number n received and 1. For example, when n is 3 it should return 1+2+3=6, and when n is 5 it should return 1+2+3+4+5=15. Fill in the gaps to make this work
    def sum_positive_numbers(n):
      if n<1:
        return 0
      return n + sum_positive_numbers(n-1)
    print(sum_positive_numbers(3))
    print(sum_positive_numbers(5))
  # output
  # 6
  # 15

  # Example
  def factorial(n):
    if n<2:
      return 1
    return n*factorial(n-1)
  factorial(1000)
  # Error because 1000 is beyond maximum recursive call limit, use recursive for smaller things

  # Example
  # Fill in the blanks to make the is_power_of function return whether the number is a power of the given base. Note: base is assumed to be a positive number. Tip: for functions that return a boolean value, you can return the result of a comparison.
  def is_power_of(number, base):
    if number<base:
      return number == 1
    return is_power_of(number/base, base)
  print(is_power_of(8,2))
  print(is_power_of(64,4))
  print(is_power_of(70,10))
  # output:
  # True
  # True
  # False

  # Example
  # In the provided code, the count_users function uses recursion to count the number of users that belong to a group within a company's system. It does this by iterating through each member of a group, and if a member is another group, it recursively calls count_users to count the users within that subgroup. However, there is a bug in the code! Can you spot the problem and fix it?
    def count_users(group):
      count = 0
      for member in get_members(group):
        count += 1
        if count_users(member) --> # change to if is_group(member) to count people who were already in a group
          count += count_users(member)-1 # minus 1 to not re-count the people in 2 groups
        return count
      print(count_users("sales"))
      print(count_users("engineering"))
      print(count_users("everyone"))
      # output:
      # 3
      # 8
      # 18

  # Example
  # In the while loops practice quiz, you were asked to write a function to calculate the sum of all positive numbers between 1 and n. Rewrite the function using recursion instead of a while loop. Remember that when n is less than 1, the function should return 0 as the answer.
  def sum_positive_numbers(n):
    if n<1:
      return 0
    return n + sum_of_positive_numbers(n-1) # add every number inside sum_of_positive_numbers and subtract 1 each time
  print(sum_of_positive_numbers(3))
  print(sum_of_positive_numbers(5))
  # output:
  # 6
  # 15
  
  
      
  


  
