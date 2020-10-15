# Troubleshooting and Debugging   

  
Troubleshooting is the process of identifying, analyzing and solving problems. Debugging specifically means solving a problem in script level or in a code.  
  
The super important thing while solving a problem is `Reproduction case`, this means A clear description of how and when the problem apprears.  
  
Commonly used troubleshooting and debugging tools are:  
1. strace: Find out system calls (call the program maked with the running kernel.   
2. ltrace: Find out the library calls in a program.   
3. top:  
  

### strace    
  
```
strace -o failure.strace ./script.py

or

strace ./script.py | less  
```  
  
### Steps of Troubleshooting  
1. Find the reproduction case.  
2. Finding the root cause.  

  
#### Reproduction Case  
A way to verify if the problem is present or not.  
We need to reproduce the error in our machine to fix it. We can ask the user when and how the problems happened and do the same.  


#### Root Cause  
Sometimes it might seem figuring out the reproduction case is the root cause. But its not always.  
Understanding the root cause is essential for long term remediation.  
We generate new hypothesis of the problem and try to test it on the test environment if possible not in the actual environment. If its not then come up with new hypothesis.   

#### Intermitten issue  
An issue that occurs ocassionally.  

#### Heisenbug issue  
Named after Warner Heisenbug, the scientist that first described the observer effect, where just observing a phenomenon alters the phenomenon.  
A heisenbug is a software bug that seems to disappear or alter its behavior when one attempts to study it.  
Sometimes, the bug goes away when we add extra logging information, or when we follow the code step by step using a debugger.  
  
> A software bug might we be dealing with if power cycling resolves a problem is cause due to Poorly managed resources.  


## Slowness   
#### Top
`top` command lets us see which processes in the computer use how much amount of resources.  

If the programm needs to access a variable declared in function it remains in `cpu`. If it is in the running programm but not in the current function it is likely in `RAM`.  
  
#### Cache:
Stores data in a form that faster to access than its original form.   
Examples:
`Web Proxy` (A web proxy is a form of cash. It stores websites, images, or videos that are accessed often by users behind the proxy)  
`DNS services` usually implement a local cache for the websites they resolve. So they don't need to query from the Internet every time someone asks for their IP address.    
  

If the application requires more memory than its available now, it put some of its resources in `swap` memory located in Disk.  
The OS constantly swap and retrieve data when required.  


#### Memory Leak  
Memory that is no longer needed is not released.  


### Slow web server  

Output of `top` command is a great to look for the behavior of the system.  
`Load avearage` denotes how much time a processor is busy in a given minute.  


#### Benchmarking an web server
```
ab -n 500 www.examplesite.com/
```  

### Writing Efficient Code  

#### Profiling

`time ./script.py` shows the time for a programm to execute.  
real result is the overall time.   
user is the time in total of running in different processor or threads which might be larger than real.  
sys is the time spent doing in system level operations.  


`cProfile` is a tool to see the number of times a function is called and how much time it takes. This can be visualized in graphical format using another tool.    
  
```
pprofile3 -f callgrind -o profiler_output.out ./script.py  
kcachegrind profiler_output.out  
```  

  
## Concurrency  
OS is in the charge to divide the fraction of CPU time for each processes running on the computer and switch between them. A computer can have multiple threads. One single core can run multiple processes at the same time in parallel. Each processes has its own memory allocation and does its own IO calls.   

Processes doesn't share memory but we might need to allow shared memory, for this reason we can use `threads`.  
This allows the threads to share some of the memory with other threads in the same process. But this is not handled by the OS so we need to wite our code to do so.  

#### Threads
Threads let us run paraller tasks inside a process.  
  
In python we can use `Threading` and `AsyncIO` modules to perform it. These modules let us spcify which parts of the code we want to execute in seperate threads or as seperate asynchronous events and how we want the result at the end.  

Sometimes threads can be executed in the same CPU processor, so in order to use more processors, we need to split the code into fully seperate processors.  

If our scrips is mostly waiting on input output or IO bounds we will also use all the processors because we will be using all the CPU time. But sometimes it make the execution much slower when IO will spend more time between switching.  

If we have processes one that use Network IO, one disk IO then we will be doing these tasks in different processors/processes.    
  

#### Executor
The process that's in charge of distributing the work among different workers.  
The `Future` module provides a couple of executors, one for using Threads and another for using Processes.    

#### Future module 
```
from concurrent import futures

# create an executor  
executor = futures.ThreadPoolExecutor()
# or 
executor = futures.ProcessPoolExecutor()

for _ in loop:
  executor.submit(func_name, param_1, param_2)

print("Waiting for all the thread to finish")
executor.shutdown()
```  


### Rsync
`rsync` is a tool for syncing files from a networked connected drive to the local computer. It doesn't compeletely copy files rather sync the chnages.  

```
import subprocess
src = "<source-path>" # replace <source-path> with the source directory
dest = "<destination-path>" # replace <destination-path> with the destination directory

subprocess.call(["rsync", "-arq", src, dest])
```    
  

## Programm Crash  
#### Invalid Memory:  
Accessing invalid memory means that the process tried to access a portion of the system's memory that wasn't assigned to it.  

#### Valgrind
A very powerful tool that can tell us if the code is doing any invalid operattions, no matter if it crashes or not.   

Someties some application doesn't have debugging sysmbols, means it removes the name of the functions to reduce its size. So these packages provide to access to download debugging symbols for debugging purpose. Ubuntu ships debugging symbol packages for all of its packages.   

> Valgrind is available on MacOS and Linux. Dr. Memory is a similar tool available on Windows and Linux.  

#### pdb3   
Python provides the debugging tool called pdb3.    


`prb3 name_of_script.py file_other_params`  

#### Off-by-one Error
Caused when the index greater than one for >= conditions.  

#### Core files
When a process crashes, the operating system may generate a file containing information about the state of the process in memory to help the developer debug the program later.  

 










































 












