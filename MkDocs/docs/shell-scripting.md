# Shell Scripting

## Shebang
```bash
#!/bin/bash
```

`#` = Sharp `!` = Bang `#!` = Shebang

## Shell-Built-in-Functions

Shell built-in functions can be identified by `type`:

```bash
type echo
type -a echo
```

## Variables

-   Case sensitive
-   Capitals are usual
-   No blanks before and after the =
-   Have to start with a letter or underscore
-   Curved brackets are not mandatory

!!! Single quotes provide the exact content, double quotes interpret the variable and provide its content.

```bash
SKILL="shell scripting"
echo "I learned ${SKILL}!"
```

-   Store the output of a command in a variable

```bash
USER_NAME=$(id -un)
USER_NAME=`id -un`    # old Syntax
```

| Variable   | Meaning                                        |
| ---------- | ---------------------------------------------- |
| $?         | Return value of the last command               |
| $!         | PID of the last started background process     |
| $\$        | PID of the current shell                       |
| $0         | filename of the currently executed script      |
| $#         | number of parameters passed to the script      |
| $1 bis $9  | Parameter 1-9                                  |
| $* oder $@ | Total of all passed parameters                 |

### System Variables

-   UID - User ID: root has always UID 0.

## if

-   Spaces after the brackets are mandatory!!

```bash
if [[ "${UID}" -eq 0 ]]
then
    echo ...
elif [[ "${UID} -eq 1 ]]
then
    echo ...
else 
    echo ...
fi
```

-   If the if-statement is written in one line, each statement has to be terminated with a ; !!
    
-   Arithmetic Tests:
    
    -   eq: is equal
    -   ne: not-equal
    -   lt: less-than
    -   le: less-than-or-equal
    -   gt: greater-than
    -   ge: greater-than-or-equal
    
    Detailed information: `help test`

- Tets if a file exists with the name specified in x
  ```bash
  test -f $x
  ```
    

## Exit-Statement

A script can be exited with `exit`. Thereby a parameter can be passed. If the script runs without problems the value 0 is returned, otherwise the value 1 (or another value not equal to 0).

```bash
exit 0
exit 1
```

The variable `${?}` contains the exit status of the last executed command.

```bash
if [[ "${?}" -ne 0 ]]
then
        echo 'The last command did not execute succesfully.'
fi
```

## Input/Output

-   `read` takes input from the user and stores it in a variable. With `p` a prompt can be specified. But with `read` also the content of a file can be stored in a variable.

```bash
read -p 'Enter a name: ' USER_NAME
read LINE < ${FILE}
```

- File descriptor:
    -   FD 0 - STDIN
    -   FD 1 - STDOUT
    -   FD 2 - STDERR

```bash
echo ${RANDOM} > ${FILE} # writes STDOUT into a file
echo ${RANDOM} >> ${FILE} # appends a file with STDOUT
read LINE < ${FILE} # reads STDIN from a file
echo ${RANDOM} 2> ${FILE} # write STDERR into a file
echo ${RANDOM} &> ${FILE} # write STDOUT and STDERR into a file
echo "This is STDERR!" >&2 # Send output to STDERR
```

- `|` normally forwards only STDOUT, STDERR is discarded. To forward also the errors `|&` has to be used.
- To get rid of STDOUT or STDERR they can be redirected to `/dev/null`.

## Create Useraccount

```bash
read -p 'Enter the username to create: ' USER_NAME
read -p 'Enter the name of the person who this account is for: ' COMMENT
read -p 'Enter the password to use for the account: ' PASSWORD

# Create the user.
useradd -c "${COMMENT}" -m ${USER_NAME}

# Set the password for the user.
# CentOS
echo ${PASSWORD} | passwd --stdin ${USER_NAME}
# Ubuntu
echo ${USER_NAME}:${PASSWORD} | sudo chpasswd
```

## Argumente und Parameter

-   Arguments are used outside the script, i.e. passed to the script. Parameters are used inside the script to reuse the passed arguments. 
- `$0` : Contains the name of the script (complete path if provided). 
- `$1` : 1. parameter
- Arguments with a space must be set in "".
- `$#` contains the number of passed arguments.
- `$?` return value of the last command.
-  `$*` or `$@` total of passed parameters.

```bash
# Make sure they at least supply one argument.
if [[ "${#}" -lt 1 ]]
then
        echo "Usage: ${0} USER_NAME [USER_NAME]..."
        exit 1
fi
```

## For Loop

```bash
for USER_NAME in "${@}"
do
        PASSWORD=$(date +%s%N | sha256sum | head -c48)
        echo "${USER_NAME}: ${PASSWORD}"
done
```
```bash
# Loop over all lines of the text file
for db in $(cat dbs.txt);do
	mysqldump $db | gzip -c > $db.sql.gz
done
```

-   `${@}` : list with all provided arguments
-   `${*}`: list with all provided arguments (ATTENTION: Have to be separated with "").

## While Loop

```bash
while [[ "${#}" -gt 0 ]]
do
    echo "Number of parameters: ${#}"
    shift
done
```

-   `shift` : discards the first parameter, the second parameter becomes the first... (N+1 → N)

## Case

```bash
case "${1}" in
    start)
        echo 'Starting.'
    ;;
  stop)
    echo 'Stopping.'
    ;;
  status|state|--status|--state)         # With a | you can give more patterns
    echo 'Status:'
    ;;
  *)                                     # All other patterns including null
    echo 'Supply a valid option.' >&2
    exit 1
    ;;
esac
```
## Arrays
- Simple Array
  ```bash
  x=()               # Definition
  x[0]='a'           # Zuweisung
  x=('a' 'b' 'c')    # Zuweisung mehrerer Parameter
  echo ${x[1]}       # auslesen
  echo ${x[@]}       # alle Array-Elemente lesen
  ```
- Associative Array
  ```bash
  declare -A y
  y[abc] = 123
  y[efg] = xxx
  y=( [abc]=123 [efg]=xxx)
  echo ${y[abc]}
  ```
- Read a text file line by line into the elements of an array. 
    ```bash
    mapfile z < textdatei
    ```


## Function
```bash
function myfunc {
	echo "Hello World, $1"
}
myfunc "Linux"
```