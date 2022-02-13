# Special Parameters

| Parameter | Meaning |
| --------- | ------- |
| $n | Positional parameter n max. n=9 ($0 is the name of the shell script) |
| ${nn} | Positional parameter nn (for nn>9) |
| $# | Number of positional parameters (not including the script program) |
| $@, $* | All positional parameters |
| $@ | Same as \"$1\" \"$2" . . . "$n\" |
| $* | Same as \"$1c$2c . . . $n\" c = content of $IFS (default is space) |
| $? | Exit status of the last command |
| $$ | Process ID of the current shell |
| $- | Current options in effect |
| $! | Process ID of the last background command |
| $- | Name of the current shell (in this case \'bash\') |

## The shift command

The shift command moves the assignment of the positional parameters to the left. If a
script is called like this:

```
script1 aaa bbb ccc ddd
```

And the following commands are run inside the script

```bash
echo $1 $2 $3
shift
echo $1 $2 $3
```

The result of the first echo command is:

```
aaa bbb ccc
```

The result of the second echo command is:

```
bbb ccc
```

## The set and unset commands

The unset command is normally used to unset values of variables, and the set command to assign values to positional parameters from inside a script. Very useful if a script has been started without positional parameters and after verifying this the script assigns default values to them.

| Command | Result |
| ------- | ------ |
| set aa bb cc dd $1 $2 $3 $4 | assigns aa to $1, bb to $2, cc to $3 and dd to $4 |

The set command is also useful for changing properties of bash's behaviour.

One important option of set is:

```bash
set -o noclobber
```

The command causes the redirection symbol (>) to fail to overwrite the contents of an existing file. 

# Conditional Expressions

The test and [...] commands are used to evaluate conditional expressions with file attributes, strings, and integers. The basic format is: test expression or [ expression ], where expression is the condition you are evaluating. There must be whitespace after the opening bracket, and before the closing bracket. Whitespace must also separate the expression arguments and operators. If the expression evaluates to true, then a zero exit status is returned, otherwise the expression evaluates to false and a non-zero exit status is returned.

## Test File Operators

| Condition | Result |
| --------- | ------ |
| -a <file> | True if file exists. |
| -b <file> | True if file exists and is a block special file. |
| -c <file> | True if file exists and is a character special file. |
| -d <file> | True if file exists and is a directory. |
| -e <file> | True if file exists. |
| -f <file> | True if file exists and is a regular file. |
| -g <file> | True if file exists and is set-group-id. |
| -h <file> | True if file exists and is a symbolic link. |
| -k <file> | True if file exists and its ``sticky bit is set. |
| -p <file> | True if file exists and is a named pipe (FIFO). |
| -r <file> | True if file exists and is readable. |
| -s <file> | True if file exists and has a size greater than zero. |
| -t <fd> | True if file descriptor fd is open and refers to a terminal. |
| -u <file> | True if file exists and its SUID bit is set. |
| -w <file> | True if file exists and is writable. |
| -x <file> | True if file exists and is executable. |
| -O <file> | True if file exists and is owned by the effective UID. |
| -G <file> | True if file exists and is owned by the effective GID. |
| -L <file> | True if file exists and is a symbolic link. |
| -S <file> | True if file exists and is a socket. |
| -N <file> | True if file exists and has been modified since it was last read. |
| file1 -nt file2 | True if file1 is newer (according to the modification date) than file2, or if file1 exists and file2 does not. |
| file1 -ot file2 | True if file1 is older than file2, or if file2 exists and file1 does not. |
| file1 -ef file2 | True if file1 and file2 refer to the same device and inode numbers. |

## Test String Operators

| Condition | Result |
| --------- | ------ |
| -n string | True if length of string is not zero |
| -z string | True if length of string is zero |
| string | True if string is not set to null |
| string1 = string2 | True if string1 is equal to string2 |
| string1 = string2 | True if string1 is equal to string2 |
| string1 = string2 | True if string1 is not equal to string2 |
| string1 < string2 | True if string1 sorts before string2 lexicographically in the current locale. |
| string1 > string2 | True if string1 sorts after string2 lexicographically in the current locale. |
| string = pattern | True if string matches pattern |
| string = pattern | True if string does not match pattern |

## Test Integer Operators

| Condition | Result |
| --------- | ------ |
| exp1 -eq exp2 | True if exp1 is equal to exp2 eg. [ "$#" -eq 4 ] |
| exp1 -ne exp2 | True if exp1 is not equal to exp2 eg. test "$#" -ne 3 |
| exp1 -le exp2 | True if exp1 is less than or equal to exp2 |
| exp1 -lt exp2 | True if exp1 is less than exp2 |
| exp1 -ge exp2 | True if exp1 is greater than or equal to exp2 |
| exp1 -gt exp2 | True if exp1 is greater than exp2 |

## Other test Operators

| Condition | Result |
| --------- | ------ |
| ! exp  | True if the given expression is false eg. [ ! -r /etc/motd ] |
| exp1 -a exp2  | True if both exp1 and exp2 evaluate to true (see example below) |
| exp1 -o exp2 | True if either exp1 or exp2 evaluate to true |
| \( exp \)  | True if exp is true; used to group expressions |

The \ used to escape parentheses. Use spaces before and after this character
```
[ "$A" = "$B" -a \( "$C" = "$D" -a "$E" = "$F" \) ] 
```
