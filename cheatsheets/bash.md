---
title: Bash scripting
category: CLI
layout: 2017/sheet
tags: [Featured]
updated: 2017-08-26
---

Getting started
---------------
{: .-three-column}

### Example

```bash
#!/usr/bin/env bash

NAME="John"
echo "Hello $NAME!"
```

### Variables

```bash
NAME="John"
echo $NAME
echo "$NAME"
echo "${NAME}!"
```

### String quotes

```bash
NAME="John"
echo "Hi $NAME"  #=> Hi John
echo 'Hi $NAME'  #=> Hi $NAME
```

### Shell execution

```bash
echo "I'm in $(pwd)"
echo "I'm in `pwd`"
# Same
```

See [Command substitution](http://wiki.bash-hackers.org/syntax/expansion/cmdsubst)

### Conditional execution

```bash
git commit && git push
git commit || echo "Commit failed"
```

### Functions
{: id='functions-example'}

```bash
get_name() {
  echo "John"
}

echo "You are $(get_name)"
```

See: [Functions](#functions)

### Conditionals
{: id='conditionals-example'}

```bash
if [ -z "$string" ]; then
  echo "String is empty"
elsif [ -n "$string" ]; then
  echo "String is not empty"
fi
```

See: [Conditionals](#conditionals)

### Strict mode

```bash
set -euo pipefail
IFS=$'\n\t'
```

See: [Unofficial bash strict mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/)

### Brace expansion

```bash
echo {A,B}.js
```

| `{A,B}` | Same as `A B` |
| `{A,B}.js` | Same as `A.js B.js` |
| `{1..5}` | Same as `1 2 3 4 5` |

See: [Brace expansion](http://wiki.bash-hackers.org/syntax/expansion/brace)


Parameter expansions
--------------------
{: .-three-column}

### Basics

```bash
name="John"
echo ${name}
echo ${name/J/j}    #=> "john" (substitution)
echo ${name:0:2}    #=> "jo" (slicing)
echo ${food:-Cake}  #=> $food or "Cake"
```

```bash
length=2
echo ${name:0:length}  #=> "jo"
```

See: [Parameter expansion](http://wiki.bash-hackers.org/syntax/pe)

```bash
STR="/path/to/foo.cpp"
echo ${STR%.cpp}    # /path/to/foo
echo ${STR%.cpp}.o  # /path/to/foo.o

echo ${STR##*.}     # cpp (extension)
echo ${STR##*/}     # foo.cpp (basepath)

echo ${STR#*/}      # path/to/foo.cpp
echo ${STR##*/}     # foo.cpp

echo ${STR/foo/bar} # /path/to/bar.cpp
```

```bash
STR="Hello world"
echo ${STR:6:5}   # "world"
echo ${STR:-5:5}  # "world"
```

```bash
SRC="/path/to/foo.cpp"
BASE=${STR##*/}   #=> "foo.cpp" (basepath)
DIR=${SRC%$BASE}  #=> "/path/to" (dirpath)
```

### Substitution

| Code | Description |
| --- | --- |
| `${FOO%suffix}` | Remove suffix |
| `${FOO#prefix}` | Remove prefix |
| --- | --- |
| `${FOO%%suffix}` | Remove long suffix |
| `${FOO##prefix}` | Remove long prefix |
| --- | --- |
| `${FOO/from/to}` | Replace first match |
| `${FOO//from/to}` | Replace all |
| --- | --- |
| `${FOO/%from/to}` | Replace suffix |
| `${FOO/#from/to}` | Replace prefix |

### Substrings

| `${FOO:0:3}`  | Substring _(position, length)_ |
| `${FOO:-3:3}` | Substring from the right |

### Length

| `${#FOO}` | Length of `$FOO` |

### Default values

| `${FOO:-val}`        | `$FOO`, or `val` if not set |
| `${FOO:=val}`        | Set `$FOO` to `val` if not set |
| `${FOO:+val}`        | `val` if `$FOO` is set |
| `${FOO:?message}`    | Show error message and exit if `$FOO` is not set |

The `:` is optional (eg, `${FOO=word}` works)

Loops
-----
{: .-three-column}

### Basic for loop

```bash
for i in /etc/rc.*; do
  echo $i
done
```

### Ranges

```bash
for i in {1..5}; do
    echo "Welcome $i"
done
```

### Reading lines

```bash
cat file.txt | while read line; do
  echo $line
done
```

### Forever

```bash
while true; do
  ···
done
```

Functions
---------
{: .-three-column}

### Defining functions

```bash
myfunc() {
    echo "hello $1"
}
```

```bash
# Same as above (alternate syntax)
function myfunc() {
    echo "hello $1"
}
```

```bash
myfunc "John"
```

### Returning values

```bash
myfunc() {
    local myresult='some value'
    echo $myresult
}
```

```bash
result=$(myfunc)
```

### Raising errors

```bash
myfunc() {
  return 1
}
```

```bash
if myfunc; then
  echo "success"
else
  echo "failure"
fi
```

### Arguments

| Expression | Description                        |
| ---        | ---                                |
| `$#`       | Number of arguments                |
| `$*`       | All arguments                      |
| `$@`       | All arguments, starting from first |
| `$1`       | First argument                     |

See [Special parameters](http://wiki.bash-hackers.org/syntax/shellvars#special_parameters_and_shell_variables).

Conditionals
------------
{: .-three-column}

### Conditions

| Condition                | Description           |
| ---                      | ---                   |
| `[ -z STRING ]`          | Empty string          |
| `[ -n STRING ]`          | Not empty string      |
| ---                      | ---                   |
| `[ NUM -eq NUM ]`        | Equal                 |
| `[ NUM -ne NUM ]`        | Not equal             |
| `[ NUM -lt NUM ]`        | Less than             |
| `[ NUM -le NUM ]`        | Less than or equal    |
| `[ NUM -gt NUM ]`        | Greater than          |
| `[ NUM -ge NUM ]`        | Greater than or equal |
| ---                      | ---                   |
| `[[ STRING =~ STRING ]]` | Regexp                |
| ---                      | ---                   |
| `(( NUM < NUM ))`        | Numeric conditions    |

| Condition          | Description              |
| ---                | ---                      |
| `[ -o noclobber ]` | If OPTIONNAME is enabled |
| ---                | ---                      |
| `[ ! EXPR ]`       | Not                      |
| `[ X ] && [ Y ]`   | And                      |
| `[ X ] || [ Y ]`   | Or                       |

### File conditions

| Condition             | Description             |
| ---                   | ---                     |
| `[ -e FILE ]`         | Exists                  |
| `[ -r FILE ]`         | Readable                |
| `[ -h FILE ]`         | Symlink                 |
| `[ -d FILE ]`         | Directory               |
| `[ -w FILE ]`         | Writable                |
| `[ -s FILE ]`         | Size is > 0 bytes       |
| `[ -f FILE ]`         | File                    |
| `[ -x FILE ]`         | Executable              |
| ---                   | ---                     |
| `[ FILE1 -nt FILE2 ]` | 1 is more recent than 2 |
| `[ FILE1 -ot FILE2 ]` | 2 is more recent than 1 |
| `[ FILE1 -ef FILE2 ]` | Same files              |

### Example

```bash
# String
if [ -z "$string" ]; then
  echo "String is empty"
elsif [ -n "$string" ]; then
  echo "String is not empty"
fi
```

```bash
# Combinations
if [ X ] && [ Y ]; then
  ...
fi
```

```bash
# Regex
if [[ "A" =~ "." ]]
```

```bash
if (( $a < $b ))
```

```bash
if [ -e "file.txt" ]; then
  echo "file exists"
fi
```

Arrays
------

### Defining arrays

```bash
Fruits=('Apple' 'Banana' 'Orange')
```

```bash
Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"
```

### Working with arrays

```bash
echo ${Fruits[0]}           # Element #0
echo ${Fruits[@]}           # All elements, space-separated
echo ${#Fruits[@]}          # Number of elements
echo ${#Fruits}             # String length of the 1st element
echo ${#Fruits[3]}          # String length of the Nth element
echo ${Fruits[@]:3:2}       # Range (from position 3, length 2)
```

### Operations

```bash
Fruits=("${Fruits[@]}" "Watermelon")    # Push
Fruits=( ${Fruits[@]/Ap*/} )            # Remove by regex match
unset Fruits[2]                         # Remove one item
Fruits=("${Fruits[@]}")                 # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}") # Concatenate
lines=(`cat "logfile"`)                 # Read from file
```

### Iteration

```bash
for i in "${arrayName[@]}"; do
  echo $i
done
```

Options
-------

### Options

```bash
set -o noclobber  # Avoid overlay files (echo "hi" > foo)
set -o errexit    # Used to exit upon error, avoiding cascading errors
set -o pipefail   # Unveils hidden failures
set -o nounset    # Exposes unset variables
```

### Glob options

```bash
set -o nullglob    # Non-matching globs are removed  ('*.foo' => '')
set -o failglob    # Non-matching globs throw errors
set -o nocaseglob  # Case insensitive globs
set -o globdots    # Wildcards match dotfiles ("*.sh" => ".foo.sh")
set -o globstar    # Allow ** for recursive matches ('lib/**/*.rb' => 'lib/a/b/c.rb')
```

Set `GLOBIGNORE` as a colon-separated list of patterns to be removed from glob 
matches.

Miscellaneous
-------------

### Numeric calculations

```bash
$((a + 200))      # Add 200 to $a
```

```bash
$((RANDOM%=200))  # Random number 0..200
```

### Subshells

```bash
(cd somedir; echo "I'm now in $PWD")
pwd # still in first directory
```

### Redirection

```bash
python hello.py > output.txt   # stdout to (file)
python hello.py >> output.txt  # stdout to (file), append
python hello.py 2> error.log   # stderr to (file)
python hello.py 2>&1           # stderr to stdout
python hello.py 2>/dev/null    # stderr to (null)
```

```bash
python hello.py < foo.txt
```

### Inspecting commands

```bash
command -V cd
#=> "cd is a function/alias/whatever"
```

### Trap errors

```bash
trap 'echo Error at about $LINENO' ERR
```

or

```bash
traperr() {
  echo "ERROR: ${BASH_SOURCE[1]} at about ${BASH_LINENO[0]}"
}

set -o errtrace
trap traperr ERR
```

### Case/switch

```bash
case "$1" in
  start | up)
    vagrant up
    ;;

  *)
    echo "Usage: $0 {start|stop|ssh}"
    ;;
esac
```

### Source relative

```bash
source "${0%/*}/../share/foo.sh"
```

### printf

```bash
printf "Hello %s, I'm %s" Sven Olga
#=> "Hello Sven, I'm Olga
```

### Directory of script

```bash
DIR="${0%/*}"
```

### Getting options

```bash
while [[ "$1" =~ ^- && ! "$1" == "--" ]]; do case $1 in
  -V | --version )
    echo $version
    exit
    ;;
  -s | --string )
    shift; string=$1
    ;;
  -f | --flag )
    flag=1
    ;;
esac; shift; done
if [[ "$1" == '--' ]]; then shift; fi
```

### Heredoc

```sh
cat <<END
hello world
END
```

### Reading input

```bash
echo -n "Proceed? [y/n]: "
read ans
echo $ans
```

```bash
read -n 1 ans    # Just one character
```

### Process IDs

| `$?` | PID of last foreground task |
| `$!` | PID of last background task |
| `$$` | PID of shell |

See [Special parameters](http://wiki.bash-hackers.org/syntax/shellvars#special_parameters_and_shell_variables).

## Also see
{: .-one-column}

* [Bash-hackers wiki](http://wiki.bash-hackers.org/) _(bash-hackers.org)_
* [Shell vars](http://wiki.bash-hackers.org/syntax/shellvars) _(bash-hackers.org)_
* [Learn bash in y minutes](https://learnxinyminutes.com/docs/bash/) _(learnxinyminutes.com)_
