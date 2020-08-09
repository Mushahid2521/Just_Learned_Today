## Managing Data and Processes
Often we need to run system command which can be executed with Python script. We also need to change Environment variables sometimes which can also be done in Python script.

### Environment Variable  
  
- **Shell:** Shell is a command line interface used to interact with Operating System.

> By writing ```env``` we can print all the environment variables in shell.  
>  
> os.environ.get("VARIABLE_NAME") using script.  
```
import os
print(os.environ.get("HOME", ""))
print(os.environ.get("SHELL", ""))
```  

> **export NEW_VARIABLE=Value** can be used to add an environment varibale.
  
 ### Command line arguments
 Parameters that are passed to the programs when it's started.   
 
 - **Exit Status:** The value returned by a program to the shell. 0 for successful and other than 0 for unsuccessful execution.   
   
 > **sys.argv** is the list containing all the command line arguments. sys.argv[0] is the script name.
  
_Example:_ Checking the file already exist or not before creating it using cmd line argument passed file name. 
  
#### Python3 and Python2 input Function
**Python2:**  
```
raw_input() -> treats input as String
input() -> make evaluation as a statement. (Ex: Input: 123+1 Output: 124)
```  
**Python3:**  
```
input() -> treats as String.
eval(input()) -> evaluate as raw_string in Python2
```  

#### Subprocess in Python
Subprocess is a Child process that halt the Parent process until the child process execution is finished. 
```
import subprocess

subprocess.run(["date"]) -> return the CompletedProcess(args=['date'], returncode=0)
subprocess.run(["sleep", 2]) -> first list item is the process name and others are the cmd line arguments needs to pass.  


result = subprocess.run(["host", "8.8.8.8"], capture_output=True)
print(result.stdout) -> result will be in stdout if successful else in stderr.  
```  

> stdout and stderr output the result as binary streams. _result.stdout.decode()_