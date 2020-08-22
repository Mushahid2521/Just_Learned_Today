## Bash Scripting and Bash Language  
Bash is the most commonly used Shell in Linux. This is not only an Interpreter that runs commands but also a scripting language.  
  
### Common Bash Commands  
> ```echo``` "something to write" >> fileName.ext   

To create an empty file. Echo is used to print something on the shell. 

> ```cat``` filename.ext  
 
Printing the content of the file in the shell.  
> ..

Parent Directory  
> ```cp``` source_directory destination_directory   

Copy file to destination.   

> ```pwd```  

Printing the Current working directory  
> ```.```  

Current directory  
> ```ls -l```   

Print all the files and folder with details information.  
> ```ls -la```  

Shows all files including hidden files that start with ```.```  
> ```rm``` file_name.ext  

Removes a single file  
> ```rm``` -r directory    

Removes the Directory including empty one.  
> ```rm *```  

Removes all the files in the directory but excluding the folders.   
> ```rmdir``` directory   

Remove an empty Directory.  

#### Redirecting Streams   
The process of sending a stream to a different destination. When we want to store the output on a file rather than printing it on the screen.  
  
> ./python_script.py > file.txt  

Store the content of the Print statements into the text file.  ```>``` overrides the content of the file.  

> ```echo``` "New Line appednded" ```>>``` file.txt  

Line appended to the file. Not overwritten.  

> ./stream.py ```<```  file.txt   

Reads the first line from the file.txt and pass it to the input functions to the Python script.   

> ./stream.py ```2>```  file.txt  

Writes the STDERR in the text file. 2> is the file descriptor. 2>> will append in the file.  

### Pipes  
Pipe connects the Output of one program to the input of another in order to pass data between programs.  

> ```ls -l | less```  

When we have many files in the directory and want to see one page at a time. Q to quite the.  

> ```cat``` spider.txt | ```tr``` ' '  '\n' | ```sort``` | ```uniq -c``` | ```sort -nr```  | ```head```  

tr command is making each word in a new line. Sort commans is then sorting the words alphabetically. Uniq command is taking unique items from the world in the lines and -c is counting the number of times it. It then again sorts the unique words in the reverse order using -nr flag. At last we can see first 10 of the result using head.   

Using ```for line in sys.stdin``` we can iterate over the line of the files. Ex: ```cat file.txt | ./capitalize.py```.  or ```./capitalize.py < file.txt```  

> When system command doesn't meet the functionality then writing the Python script to meet the gap and include it in the pipeline.  

### Signals  
Tokens delivered to running processes to indicate a desired action. This can be used to call the process to take a particular action without interrupting the process. This could be leading a disk storage and reconfiguring the resources etc.   


```ping www.example.com```  
```CTR + C``` => finishes the program politely. It is ```SIGINT``` type signal. 
```CTR + Z``` => this stops the program to run without terminating. 
```fg``` => restart the paused program to start running again.   

#### Killing a process  
```ps -ax```   list all the running process with PID (Process Identifier)  
```ps -ax | grep ping``` find the ping process.  
```kill PID``` Kills the process defined by PID  

#### Variable 
```
example=Hello
echo $example
```  
> Variable should be without space in between.   
```
line="----------------"
echo "Starting at: $(date) echo $line 
```  

#### Condition   
In Bash scripting the condition is based on the exit status.  
An exit value 0 means success.   
```
#!/bin/bash

if grep "127.0.0.1" /etc/hosts; then
    echo "Everything Ok"
else 
    echo "ERROR! 127.0.0.1 not in found /etc/hosts"
fi
```  

```echo *.py, echo ?????.py etc```  
Similar things can be achieved in python using Glob module.  
  
#### Test
A command that evaluates the conditions received and exits with zero when they're true and with one when they're false.   

```test -n &PATH```  
Checks if the PATH variable is empty or not.    

```[ -n $PATH ]```
[] is an alias to the test command.   

#### While loop  

```
#!/bin/bash 

n=1
while [ $n -le 5 ]; do
    echo "Iteration number $n"
    ((n+=1))
done
```  

Ex: Retrying a process   
```
#!/usr/bin/env python3 

import sys
import random

value = random.randint(0,3)
print("Returning Value "+str(value))
sys.exit(value)
```

```
#!/bin/bash

n=0
command=$1
while ! $command && [ $n -le 5 ]; do
    sleep $n
    ((n+=1))
    echo "Retry #$n"
done
```  

```./retry.sh ./random-exit.py```  

Looping through elements    
```
#!/bin/bash

for fruit in peach orange apple; do
    echo "I like $fruit"
done
```  


> To get the base name of a file without extension   
>basename filename.ext ext  
  
Before performing any actions with the files its safe to check what is going to happen. To do this we can print the action to be taken.  
Example:  
```
#!/bin/bash 

for file in *.HTM; do
    name=$(basename "$file" .HTM)
    echo mv "$file" "$name.html"
done
```

Example:    
```
tail /var/log/syslog | cut -d ' ' -f5-
```  
Split the log line with spaces. -f5- tells that to take everything after 5th part.    
```cut -d' ' -f5- /var/log/syslog | sort | uniq -c | sort -nr | head```  


### Choosing Between Bash and Python  
- When the bash command line starts to becoming complex its better to idea to write a Python script that handles data in a more ```readable``` and ```testable``` way.  
- Bash can be run quick on Linux machine but not applicable in Windows.  
- Bash is not flexible and robust as like Python with many string functions, list dictionaries.    


#### Bash CheatSheet   
![first](/Images/bash_1.jpg)  
![second](/Images/bash_2.jpg)  
![third](/Images/bash_3.jpg)  
![fourht](/Images/bash_5.jpg)