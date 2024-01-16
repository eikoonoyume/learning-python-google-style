# CSV = Comma Separated Values, plaintext, each line is a single data record, each field separated by comma
cat csv_file.txt
Sabrina Green,802-867-5309,System Administrator
Eli Jones,684-3481127,IT Specialist
Melody Daniels,846-687-7436,Programmer
Charlie Rivera,698-746-3357,Web Developer

# CSV Module
# Opening File
import csv
  f = open("csv_file.txt") # opening same as usual despite "f" and not "file"
# Parse File with CSV Module
  csv_f = csv.reader(f)
# Iterate Through Contents
  for row in csv_f:
# current fields
    name, phone, role = row
# print
    print("Name: {}, Phone: {}, Role: {}".format(name, phone, role))
# Close File
f.close()

# output:
# Name: Sabrina Green, Phone: 802-867-5309, Role: System Administrator
# Name: Eli Jones, Phone: 684-3481127, Role: IT specialist
# Name: Melody Daniels, Phone: 846-687-7436, Role: Programmer
# Name: Charlie Rivera, Phone: 698-746-3357, Role: Web Developer

# Writing CSV Content for File
# Store Data Into Lists
hosts = [["workstation.local", "192.168.25.46"], ["webserver.cloud", "10.2.5.6"]]

# With Block to not forget to Close
with open('hosts.csv', 'w') as hosts_csv:
# Call Writer Function with file as parameter
writer = csv.writer(hosts_csv)
# writerows() method because we have all data
writer.writerows(hosts)

# writerow() method if you want to write one row at a time
# output:
# cat hosts.csv
# workstation.local.192.168.25.46
# webserver.cloud.18.2.5.6

# Reading and Writing CSV Files with Dictionaries
# Too many columns = names of columns will be first line in file
cat software.csv 
# Output name,version,status,users
# MailTree,5.34,production,324
# CalDoor,1.25.1,beta,22
# Chatty Chicken,0.34,alpha,4

# DictReader = reader provided by CSV modul that turns each row of data into a dictionary
# Open File
with open('software.csv') as software:
# create DictReader to process data
reader = csv.DictReader(software)
# iterate through to access keys
  for row in reader:
    print(("{} has {} users").format(row["name"], row["users]))
# output:
# MailTree has 324 users
# CalDoor has 22 users
# Chatty Chicken has 4 users

# DictWriter to write/generate a CSV file from contents of a list of dictionaries, each element = row, values will come from each dictionary
# List of Dictionaries
users = [ {"name": "Sol Mansi", "username": "solm", "department": "IT infrastructure"},
 {"name": "Lio Nelson", "username": "lion", "department": "User Experience Research"},
 {"name": "Charlie Grey", "username": "greyc", "department": "Development"}]

 # Define list of keys
 keys = ["name", "username", "department"]
 # open file
 with open('by_department.csv', 'w') as by_department:
 # create DictWriter
   writer = csv.DictWriter(by_department, fieldnames = keys)
  # writeheader() method creates first line of CSV based on keys
    writer.writeheader()
  # writerows() method to turn list of dictionaries into lines of file
    writer.writerows(users)
# output:
# cat by_department.csv
# name,username,department
# Sol Mansi,solm,IT Infrastructure
# Lio Nelson,lion,User Experience Research
# Charlie Grey,greyc,Development
