# Using a Virtual Environment in Python
python -m venv myenv

# Activating Virtual Environment: Windows command
myenv\Scripts\activate

# Activating Virtual Environment: macOS, Linux command
myenv/bin/activate

# Document and Manage Project Dependencies: create requirements.txt to list all libraries and their versions
pip freeze > requirements.txt

# Install them in new environment
pip install -r requirements.txt

# Computer Health Check: Automation Example: using shutil module and disk_usage function
import shutil
>>> du = shutil.disk_usage("/")
>>> print(du)
usage(total=255374594048, used=115190444032, free=140184150016)
>>> du.free/du.total*100
54.89353807436738

# psutil module to check cpu_usage
import shutil
import psutil
def check_disk_usage(disk):
    du = shutil.disk_usage(disk)
    free = du.free / du.total*100
    return free > 20
def check_cpu_usage():
    usage = psutil.cpu_percent(1)
    return usage < 75
if not check_disk_usage("/") or not check_cpu_usage():
    print("Error!")
else:
    print("Everything is ok!")

