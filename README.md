# assignment-10
# 10-1 and 10-2

from pathlib import Path

path = Path("learning_python.txt")

# Read entire file, print once
contents = path.read_text()
print("--- Entire file ---")
print(contents)

# Split into lines, loop over list and print again
print("\n--- Line by line (list) ---")
lines = contents.splitlines()
for line in lines:
    print(line)

# 10-2: replace 'Python' with 'C' and print modified lines
print("\n--- Replacing 'Python' with 'C' ---")
for line in lines:
    print(line.replace("Python", "C"))
# 10-3

from pathlib import Path

path = Path("learning_python.txt")
for line in path.read_text().splitlines():
    print(line)
# 10-4

from pathlib import Path

name = input("What is your name? ")
Path("guest.txt").write_text(name)
# 10-5

from pathlib import Path

path = Path("guest_book.txt")

print("Enter names for the guest book; enter 'quit' to stop.")
while True:
    name = input("Name: ")
    if name.lower() == "quit":
        break
    print(f"Welcome, {name}!")
    path.write_text(path.read_text() + name + "\n" if path.exists() else name + "\n")
# 10-6

try:
    a = input("First number: ")
    b = input("Second number: ")
    result = int(a) + int(b)
except ValueError:
    print("Please enter only numbers.")
else:
    print(f"The sum is {result}.")
# 10-7

print("Enter two numbers to add (or 'q' to quit).")
while True:
    a = input("First number: ")
    if a.lower() == "q":
        break
    b = input("Second number: ")
    if b.lower() == "q":
        break
    try:
        result = int(a) + int(b)
    except ValueError:
        print("Please enter only numbers.")
    else:
        print(f"The sum is {result}.")
# 10-8

filenames = ["cats.txt", "dogs.txt"]

for filename in filenames:
    try:
        with open(filename, "r") as f:
            print(f"\nContents of {filename}:")
            for line in f:
                print(line.rstrip())
    except FileNotFoundError:
        print(f"Sorry, {filename} is missing.")
# 10-9

filenames = ["cats.txt", "dogs.txt"]

for filename in filenames:
    try:
        with open(filename, "r") as f:
            for line in f:
                print(line.rstrip())
    except FileNotFoundError:
        pass  # fail silently
# 10-10

from pathlib import Path

filenames = ["book1.txt", "book2.txt", "book3.txt"]

for filename in filenames:
    path = Path(filename)
    if not path.exists():
        print(f"{filename} not found, skipping.")
        continue
    text = path.read_text(encoding="utf-8")
    lower_text = text.lower()
    count_the = lower_text.count("the")
    count_the_space = lower_text.count("the ")
    print(f"\nIn {filename}:")
    print(f"'the' appears about {count_the} times.")
    print(f"'the ' (with space) appears about {count_the_space} times.")

    # favorite_number_write.py

from pathlib import Path
import json

number = input("What is your favorite number? ")

path = Path("favorite_number.json")
contents = json.dumps(number)
path.write_text(contents)

print("Thanks! I'll remember that number.")
# favorite_number_read.py

from pathlib import Path
import json

path = Path("favorite_number.json")
contents = path.read_text()
number = json.loads(contents)

print(f"I know your favorite number! It's {number}.")

# favorite_number_remembered.py

from pathlib import Path
import json

path = Path("favorite_number.json")

if path.exists():
    contents = path.read_text()
    number = json.loads(contents)
    print(f"I know your favorite number! It's {number}.")
else:
    number = input("What is your favorite number? ")
    contents = json.dumps(number)
    path.write_text(contents)
    print("Thanks, I'll remember that.")
# user_dictionary.py

from pathlib import Path
import json

path = Path("user_info.json")

# Collect info from the user
username = input("What is your name? ")
age = input("How old are you? ")
favorite_color = input("What is your favorite color? ")

user_info = {
    "username": username,
    "age": age,
    "favorite_color": favorite_color,
}

# Store the dictionary as JSON text
contents = json.dumps(user_info)
path.write_text(contents)

# Read it back in
stored_contents = path.read_text()
stored_info = json.loads(stored_contents)

print("\nHere is what I remember about you:")
print(f"Name: {stored_info['username']}")
print(f"Age: {stored_info['age']}")
print(f"Favorite color: {stored_info['favorite_color']}")
# verify_user.py

from pathlib import Path
import json


def get_stored_username(path):
    """Get stored username if available."""
    if path.exists():
        contents = path.read_text()
        username = json.loads(contents)
        return username
    return None


def get_new_username(path):
    """Prompt for a new username and store it."""
    username = input("What is your name? ")
    contents = json.dumps(username)
    path.write_text(contents)
    return username


def greet_user():
    """Greet the user by name, verifying it's correct."""
    path = Path("username.json")
    username = get_stored_username(path)
    if username:
        correct = input(f"Are you {username}? (y/n) ")
        if correct.lower() == "y":
            print(f"Welcome back, {username}!")
            return
        # Username was stored but is not correct; get a new one.
        username = get_new_username(path)
        print(f"We'll remember you when you come back, {username}!")
    else:
        # No stored username yet.
        username = get_new_username(path)
        print(f"We'll remember you when you come back, {username}!")


greet_user()

