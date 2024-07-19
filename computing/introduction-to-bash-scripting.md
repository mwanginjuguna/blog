# The Basics of BASH Scripting - Bourne Again SHell `>$ ./shell.sh`

Here is a list of commands that are commonly used in running bash scripts. The commands are ordered from Basic to Advanced.

![](https://github.com/mwanginjuguna/public-image-assets/blob/main/blog/bash-scripting.png?raw=true)

## Basics

```bash
which $BASH
```
output: `/bin/bash`

Use echo to print to terminal (output)

```bash
echo "Hi there!."
```

Use `#!/bin/bash` at the top of every bash script to save a bash executable

`nano` is a poweful ide for bash

`sleep 3` to sleep 3 seconds.

```bash
nano ls -l
# - to see permissions e.g rw-r--r-- for read and write

chmod +x hithere.sh
# - to add execute permission to files

./hithere.sh
# - to run the file with execute permissions
```

## Bash Variables and Arguments/Parameters

### Variables

In bash, variables are used to store values. There are two types of variables:

Scalar variables: Store a single value.
Array variables: Store multiple values.
Declaring Variables

To declare a variable, use the = operator:

```bash
MY_VAR="Hello, World!"
```

Using Variables

To use a variable, prefix it with the $ symbol:

```bash
# use a variable
echo $MY_VAR

variableName="Mr Cool Variable"

echo "Good morning $name!"

# Output: Good morning Mr Cool Variable!"
```

**Use read to access user typed input**.

```bash
echo what is your name?

read name
```

*Let us assume the user types `>_ Username` on the terminal*

```bash
echo "What'up $name!"
```

> Output: `What'up Username!`

#### Positional parameter and variables access input or argument via the command line

These are used to access parameters passed when calling the file

```bash
./call-file.sh Malaika Mrembo
```

This command will run a file called `callme.sh` and pass the parameters `Malaika` and `Mrembo`.

```bash
# filename callme.sh
name=$1
complement=$2

echo "Jambo $name!! Wewe ni $complement."
```

> output: `Jambo Malaika!! Wewe ni Mrembo.`

```bash
# who am I (current logged in user)? 
whoami # > output - root

# print current working directory (where am I?)
pwd

# print current date (dateTime format timestamp UTC)
date # output: Thu Jan 31 01:53:04 PM UTC 2024

```

**How to use commandline output as a variable or parameter**.

```bash
# Saving the name of the currently logged in user in a variable
user=$(whoami)
$date =$(date)

echo "You are currently logged in as $user. The time is $date."
```

**Bash has built-in variables.**

```bash
echo $USER # > output: root (currently logged in user)

echo $SHELL # > output: /bin/bash

echo $PWD # > output: /root (print current working directory)

echo $HOSTNAME # > output: localhost

```

**Create variables and use them in child files using `export`.**

```bash

$twitter="Elon Musk's X!"

export twitter

# these variables will be lost on exit
```

**Make the variables permanent.**

```bash
ls -al provides details of files in a directory with permissions and users details
```

`.bashrc` is a script that runs on each startup.

```bash
# file name: .bashrc

# add the following at the end of the file

export twitter="Elon Musk X!"
```

- logout the current session

Next, call the variable `echo $twitter` and the output will be `Elon Musk`. So, the output has been persisted over two sections. Now, it is permanent.

**`$RANDOM` is a built-in variable that returns a random number upto 32767**.

```bash
echo $RANDOM
```

## Basic Operators

Bash provides various operators for performing arithmetic, comparison, and logical operations.

### Arithmetic Operators

`+` (addition)
`-` (subtraction)
`*` (multiplication)
`/` (division)
`%` (modulus)

### Comparison Operators

`==` (equal to)
`!=` (not equal to)
`<`(less than)
`>`(greater than)
`<=` (less than or equal to)
`>=` (greater than or equal to)

### Logical Operators

`&&` (and)
`||` (or)
`!` (not)

## Control Structures

Control structures are used to control the flow of a script.

### If-Else Statements

```bash
if [ condition ]; then
  # code to execute if condition is true
else
  # code to execute if condition is false
fi
```

### For Loops

```bash
for var in values; do
  # code to execute for each value
done
```

### While Loops

```bash
while [ condition ]; do
  # code to execute while condition is true
done
```

## Functions

Functions are reusable blocks of code that can be called multiple times in a script.

### Declaring Functions

```bash
my_function() {
  # code to execute
}
```

### Calling Functions

```bash
my_function
```

## Input/Output

Bash provides various ways to interact with the user and display output.

### Reading Input

```bash
read -p "Enter your name: " NAME
```

### Displaying Output

```bash
echo "Hello, $NAME!"
```

## File Manipulation

Bash provides various commands for manipulating files and directories.

### Creating Files

```bash
touch myfile.txt
```

### Deleting Files

```bash
rm myfile.txt
```

### Moving Files

```bash
mv myfile.txt mydir/
```

### Copying Files

```bash
cp myfile.txt mydir/
```

## Conditional Statements

Conditional statements are used to execute code based on certain conditions.

### If-Then Statements

```bash
if [ -f myfile.txt ]; then
  echo "File exists!"
fi
```

### Case Statements

```bash
case $MY_VAR in
  "value1")
    # code to execute for value1
    ;;
  "value2")
    # code to execute for value2
    ;;
  *)
    # code to execute for default
    ;;
esac
```

### Regular Expressions

Bash supports regular expressions for pattern matching.

```bash
if [[ $MY_VAR =~ ^[a-zA-Z]+$ ]]; then
  echo "Variable contains only letters!"
fi
```

## Cool Bash Scripting Techniques

**1. Parameter Expansion**

Parameter expansion allows you to manipulate variables using various operators.

Example

```bash
MY_VAR="hello world"
echo ${MY_VAR^^}  # outputs "HELLO WORLD"
```

**2. Command Substitution**

Command substitution allows you to execute a command and store its output in a variable.

**Example**

```bash
MY_VAR=$(ls -l)
echo $MY_VAR
```

**3. Process Substitution**
Process substitution allows you to treat the output of a command as a file.

**Example**

```bash
diff <(ls -l) <(ls -l /mydir)
```

**4. Arrays**
Arrays are used to store multiple values in a single variable.

**Example**

```bash
MY_ARRAY=("value1" "value2" "value3")
echo ${MY_ARRAY[0]}  # outputs "value1"
```

**5. Bashisms**
Bashisms are features specific to Bash that can make your scripts more efficient.

**Example**

```bash
MY_VAR="hello world"
echo ${MY_VAR:0:5}  # outputs "hello"
```

## Common Bash Scripting Commands

- `cd`
Change directory.

- `mkdir`
Create a new directory.

- `rm`
Delete a file or directory.

- `cp`
Copy a file or directory.

- `mv`
Move or rename a file or directory.

- `echo`
Display output to the console.

- `grep`
Search for patterns in files.

- `find`
Search for files based on various criteria.

- `sed`
Stream editor for manipulating text.

-  `awk`
Pattern scanning and processing language.

### Bash-Math

**Basic Math!**

```bash
echo $((2 + 3))

echo $(( 3/4)) # output: 0

echo $(( 5 % 4)) # output: 1

echo "What is your age?"
read $name

echo $(( $name % 5)) # output: 2

```
