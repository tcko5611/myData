else# Variables

### change backspace key

```bash
stty erase '^?'
```

# String operation

-   da.log{,.bak}→ da.log da.log.bak
-   da.log{.bak,}→ da.log.bak da.log
-   {oct,hex,dec} → oct hex dec
-   {a..f}.txt → a.txt b.txt...
-   {1..4} →1 2 3 4
-   {$aa..$bb} → {1..4}
-   ${#string} → length of string, 6
-   ${string:position:length} → substring
-   ${string#substring} → shortest substring from head
-   ${string%substring}→shortest substring from tail
-   ${string##substring} → longest substring from head
-   ${string\%\%substring}→longest substring from tail
-   ${string/pattern/replace} → from head, only one
-   ${string//pattern/replace} → from head, all

# Input

-   read varName: Read input from the user and store it in the variable varName.
-   /dev/stdin: A file you can read to get the STDIN for the Bash script

# Arithmetic

-   let expression: Make a variable equal to an expression. let a=5+4 #9
-   expr expression: print out the result of the expression. expr 5 + 4 # 9, expr 5+4 # 5+4
-   $(( expression )): Return the result of the expression. a=$(( 4 + 5 )) # echo $a # 9
-   ${#var}:Return the length of the variable var. # b=4953 echo ${#b} # 4

# if statements

-   if: Perform a set of commands if a test is true.
-   else: If the test is not true then perform a different set of commands.
-   elif: If the previous test returned false then try this one.
-   &&: Perform the and operation.
-   ||: Perform the or operation.
-   case: Choose a set of commands to execute depending on a string matching a particular pattern.

# Test expression

-   ! EXPRESSION: The EXPRESSION is false.
-   -n STRING: The length of STRING is greater than zero.
-   -z STRING: The lengh of STRING is zero (ie it is empty).
-   STRING1 = STRING2: STRING1 is equal to STRING2
-   STRING1 != STRING2: STRING1 is not equal to STRING2
-   INTEGER1 -eq INTEGER2: INTEGER1 is numerically equal to INTEGER2
-   INTEGER1 -gt INTEGER2: INTEGER1 is numerically greater than INTEGER2
-   INTEGER1 -lt INTEGER2: INTEGER1 is numerically less than INTEGER2
-   -d FILE: FILE exists and is a directory.
-   -e FILE: FILE exists.
-   -r FILE: FILE exists and the read permission is granted.
-   -s FILE: FILE exists and it's size is greater than zero (ie. it is not empty).
-   -w FILE: FILE exists and the write permission is granted.
-   -x FILE: FILE exists and the execute permission is granted.

# Loops

-   while do done: Perform a set of commands while a test is true.

```bash
input=/remote/tw_rnd1/ktc/work_fault/customfault/src/guiAdv/usedFiles
while IFS= read -r line # IFS= ... avoid trimmed of spaces???
do
    file=$prefix$line
    if [ -z $line ]
    then
        continue
    fi
    if [ -f $file ] || [ -d $file ]
    then 
        str="s/.*\\/\\(.*\\)/\\1/g"
        #echo $str
        newFile=`echo "$file" | sed -e "$str"`
        newFile=$PWD/$newFile
        echo $file
        echo $newFile
        ln -s $file $newFile
    fi
done < "$input"
```

-   until do done: Perform a set of commands until a test is true.
-   for do done: Perform a set of commands for each item in a list.
-   break: Exit the currently running loop.
-   continue: Stop this iteration of the loop and begin the next iteration.
-   select do done: Display a simple menu system for selecting items from a list.

# Functions

- function \<name> or \<name> (): Create a function called name.
- return \<value>: Exit the function with a return status of value.
-   local \<name>=\<value>: Create a local variable within a function.
-   command <command>: Run the command with that name as opposed to the function with the same name.

```bash
build()
{
    cd Debug
    /depot/cmake-3.6.2/bin/cmake -DCMAKE_BUILD_TYPE=Debug ..
    make -j 4
}
case $1 in
    rebuild) rebuild
             ;;
    build) build
           ;;
    clean) clean
           ;;
    *) echo "$1: unknown"
esac
```

# login shell and non login shell

-   login interactive shell: first process executes under your user ID when you log in for an interactive session.
    -   Bourne shell: read _/etc/profile_
    -   bash: read addtional file _/etc/profile_, _~/.bash_profile_, _~/.bash_login_ and _~/.profile_
    -   csh: read _/etc/csh.login_, ~/.login* ~/.cshrc
    -   tcsh: read _/etc/csh.cshrc_, _/etc/csh.login_, _~/.tcshrc_, _~/.cshrc_, _~/.history_, _~/.login_
-   non-login interactive shell: start a shell in an existing session.
    -   bash: read _/.bashrc_
    -   csh: read _/etc/csh.cshrc_ and _~/.cshrc_
    -   tcsh: read _/etc/csh.cshrc_ and _~/.tcshrc_ or _~/.cshrc_
-   non-interactive shell:
    -   search file in ENV BASH_ENV

# Typical use of bash

## use array

```
declare -a arr
arr[0]=$line
arr[1]="aa"
echo ${arr[0]}
echo ${arr[1]}

```

## use of utility result

```
aa=`bc <<< "scale=2; (${arr[1]} - ${arr[0]}) / ${arr[1]}"`

```

## test for float variable

```
if (( $(echo "$aa < 0.1" | bc -l) ))
then
  ...
else
  ...
fi

```