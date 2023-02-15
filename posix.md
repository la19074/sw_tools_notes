# POSIX

## Introduction

### Common POSIX compatible shells

* sh (Bourne shell)  
* bash  
* zsh  

### Shell Tricks

**TAB**: complete command or filename  
**DOUBLE TAB**: show list of possible completions  
**UP/DOWN**: scroll through history  
**^R text**: search history for command  

## Basic Commands

#### which

```sh
$ which ls
/bin/ls
```

#### ls

```sh
$ ls
file1        file2
$ ls –l
-rwx------    1 vagrant ... 40 ... file1
-rwxr-----    1 vagrant ... 80 ... file2
$ ls –a
.         ..        file      file2
```

#### help

```sh
$ ls --help
BusyBox v1.30.1 multi-call binary.

Usage: ls [-1AaCxdLHRFplinshrSXvctu] [-w WIDTH] [FILE]
```

#### man [SECTION] COMMAND

#### cp [-rfi] SRC...DEST
copy files  
-r recursive  
-f overwrite readonly  
-i ask before overwriting (interactive)

#### mv [–nf] SRC... DEST
move files  
-n no overwrite  
-f force overwrite

#### find DIR[EXPRESSION]
find all files in directory (recursively) that match an expression  
e.g. `find . –name "a\*"`

### Shell Expansion

**\***: all filenames in current scope  
e.g. a\* is filenames starting with a etc.  

**?**: single character in filename  
e.g. image???.jpg matches image001.jpg  

**\[ab\]**: single character in list  
e.g. image\[0-9\].jpg  

**\$**: variable name expansion

### Shell Quoting

**"double quotes"**: turn off pattern matching, keeps variable interpolation and backslashes on  
**'single quotes'**: turn off everything  
**\\\*, \?, \\\[, \\\$**: do not treat as pattern

## Package Managers

Package managers are used to install and manage packages and their dependancies.

**apk**: Alpine package manager, uses .apk files (same ending as Android, different format and system)  
**apt**: Debian/Ubuntu/Mint/...  
**brew**: MacOS  

### APK Commands

#### Finding Packages

`$ apk search [-v] [-d] STRING`

`$ apk info [-a] PACKAGE`

`$ apk list [-I] PACKAGE`

`$ apk [COMMAND] --help`

#### Installing

`$ sudo apk add PACKAGE [PACKAGE...]`

#### Update and Upgrade

`$ sudo apk update`

`$ sudo apk upgrade`

## Pipes

Unix programs communicate via text streams. Each program has three I/O streams:  
0 = standard input  
1 = standard output  
2 = standard error  

```sh
$ ls -1 | head
```

This pipes the output from `ls` to the input of `head`.

### Redirects

```sh
$ sort < infile > outfile
```

This redirects the contents of infile to input and output to outfile. 

`COMMAND > FILE` overwrites FILE.  
`COMMAND >> FILE` appends to FILE.  
`COMMAND <<< "STRING"` takes a string and places it on standard input.  

```sh
$ COMMAND > FILE 2> FILE2
```

This will redirect output to FILE and error to FILE2. 

```sh
$ COMMAND > FILE 2>&1
```

This will redirect both output and error to FILE. 

```sh
$ COMMAND > /dev/null
```

This will ignore the output. 

A program that expects a filename can be told to use standard input/output instead with filename `-` or `/dev/stdin` depending on support.

### Subshell

```sh
$ PROGRAM <(SOMETHING)
```

This will pipe the output of SOMETHING as a file into PROGRAM, i.e. `cat <(echo "Hi")`.

```sh
$ COMMAND $(SOMETHING)
```

This will pipe the output of SOMETHING as an argument into COMMAND.

## More Commands

### head [-n NUM]

Writes the first NUM (10 by default) lines to standard out. 

### tail [-n NUM]

Writes the last NUM (10 by default) lines to standard out. 

### grep PATTERN FILENAMES

Global regular expression parser.

### sort

Read all lines into buffer, sort, output.

### uniq

Remove adjacent duplicates. Best used as: `$ command | sort | uniq`

### tee FILE

Takes a filename as argument and writes a copy of input to it, as well as to stdout.

### less

`less` is a **pager**: it displays text on your screen, one page at a time.

* Up/Down arrows scroll
* Space/Enter advance a page
* / (forward slash) opens a search
* q quits

### sed

`sed` stands for stream editor – it can change text using a regular expression as it passes from input to output.

`s/ONE/TWO/[g]` replaces the first match for ONE (all matches, with /g) with TWO. Regular expressions are supported. 

## Shell Scripting

<https://www.shellcheck.net> is a good tool to spot unportable/dangerous things in shell scripts.

A shebang `#!` tells the shell what program to interpret the script with. 

`#! /bin/sh/` is the most portable, but you can use bash too `#! /usr/bin/env bash`.

`chmod +x FILE` makes the file executable. 

### env

`#! /usr/bin/env python3` is used for Python scripts. Why?

Programs can be located in many places like `/bin` or `/usr/bin` or `/usr/local/bin` and so on. 

`env` looks through `PATH` and tries to find the program. 

`PATH` is an environment variable that tells the system where all the programs are.

`export PATH="${PATH}:/extra/directory/to/search"` will add an extra directory to `PATH`. 

### Syntax

#### Basic

`A; B` run A then run B

`A | B` run A and feed its output as the input to B

`A && B` run A and if successful run B

`A || B` run A and if not successful run B

#### Variables

```sh
GREETING="Hello World!"
```

This creates a variable. 

```sh
echo "${GREETING}"
```

To use a variable. There’s no penalty for using one thats not defined, but `set -o nounset` will force undefined errors. 

```sh
export GREETING
```

If you want your variable to exist in the programs you start as an environment variable. 

```sh
unset GREETING
```

Unset a variable. 

##### Standard Variables

`${0}` Name of the script

`${1}, ${2}, ${3}...` Arguments passed to your script

`${#}` The number of arguments passed to your script

`${@}` and `${*}` All the arguments

`${?}` 1 byte exit value after every command (program). 0: success, >0: failure

#### If Statement

```sh
# Or [ -x myscript.sh ];
if test -x myscript.sh; then
  ./myscript.sh
fi
```

#### For Loop

##### Example 1

```sh
for file in *.py; do
  python "${file}"
done
```

Runs all python files. 

##### Example 2

```sh
for n in 1 2 3 4 5; do
  echo -n "${n} "
done
```

1 2 3 4 5

##### Example 3

```sh
seq 5
```

1 2 3 4 5

```sh
for n in $(seq 5); do
  echo -n "${n} "
done
```

1 2 3 4 5


##### Example 4

```sh
seq -s, 5
```

1,2,3,4,5

```sh
# IFS = In Field Separator
IFS=','
for n in $(seq -s, 5); do
  echo -n "${n} "
done
```

1 2 3 4 5

#### Case Statement

```sh
# Remove everything upto the last / from ${SHELL}
case "${SHELL*/}" in
  bash) echo "I'm using bash!" ;;
  zsh)  echo "Ooh fancy a zsh user!" ;;
  fish) echo "Something's fishy!" ;;
  *)    echo "Ooh something else!" ;;
esac
```

## Even More Commands

### basename "PATH"

Remove everything up to the last `/`.

```sh
for f in *.jpg; do
  convert "${f}" "$(basename "${f}" .jpg).png"
done
```

This converts all .jpg files to .png files. 

### dirname "PATH"

Get everything up to the last `/`.

### ps -A

List all processes. 


### Others: awk, ghead

See man pages. 