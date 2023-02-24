# Variables

-   **$0**: the bash shell script name
-   **$1, $2...**: the arguments of command line
-   **$#**: the number of arguments(not include $0)
-   **$@**: the all arguments of the command line
-   **$?**: the exit status of the most rrecently process
-   **\$$**: the process ID of current process
-   **var1=Hello**: set varibale var1 as 'Hello'
-   **'**: don't change te string inside it
-   **"**: will replace variable for inside string
-   **export**: will let other process use the variable
-   **variable=$(command)**: save the output of a command into a variable.