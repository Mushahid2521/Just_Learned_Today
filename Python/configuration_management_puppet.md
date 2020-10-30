# Configuration Management  

An automation technique to which lets us manage the configurations at scale.  

**Scale:**  Being able to scale means that we can keep achieving larger impact with same amount of effort.  
A scalable system is a flexible one.  

**Automation** is an essential tool for keeping up with the infrastructure needs of a growing business.  

### Features of Configuration management
1. Configuration management have some automatic error correction built in so that they can recover from certain types of errors all by themselves.  
2. If a user  changes the settings on their machine, it can detect  the changes and reapply the settings defined in the code.   

### Examples of Conf management systems  
1. Puppet
2. Chef 
3. Ansible
4. CFEngine   


**Infrastructure as Code:** The paradigm of storing all the configuration for the managed devices in version controlled files is known (IaC).  

## Puppet  
Puppet is a cross-platform and its been from 2005.   

Puppet is typically deployed in Client-Server architecture. The client is the puppet agent and the service is known as the puppet master.  
The puppet agent connects to the master and sends the state of the system and the master than process this information and generates the list of rules that need to be applied on the device and sends this list back to the agent. The agent then make necessary changes in it.  

**Example**  
```commandline
class sudo {
    package { 'sudo':
        ensure => present,
    }
}
```  

**Resource:** In puppet the, resources are the basic unit for modeling the configuration that we want to manage.  
Above `package` is a resource. Each resource specifies one configuration we want to manage.  
**Example:** package, file, service etc    

After the resource type we have `resource name` . Above `sudo` is the name of package resource.  

```
class timezone {
    file { '/etc/timezone':
        ensure => file, 
        content => "UTC\n"
        replace => true,        
    }
}
```  
Here we explicitly say that this will be a file instead of directory or symlink  

we set the content of the file to the UTC timezone  

Finally with replace which means that the contents of the file will be replaced even if the file already exists.  
  
**Domain specific language(Puppet) vs General Purpose language(Python, Java, ..)**  
  
### Facts
Variables that represent the characteristics of the system.  
When puppet agent runs, it calls a program called factor which analyzes the current system, storing the information gathered in those facts.  


> Puppet is a declarative language.  
  
we call it declarative as we declare the state we want rather than telling the steps to reach the state.  
On the other hand Python, C are procedural language.  

> Operations of Configurations management system should be Idempotent.  
  
An idempotent action can be performed over and over again without changing the system after the first time the action was performed and with no unintended side effects.  

```
file { 'etc/issue':
    mode => '0644'
    content => "Internal system \l \n"
}
```  
If the file already exists and has the desired content, then Puppet will understand that no action needs to be taken.  
if doesn't exists it will create it.  
If the contents or the permission don't match, Puppet will fix them.  

If the script is idempotent then the failure can continue from half-way from where it left off.   
The actions taken by the `exec` resource might not be the same running again.  

```
exec { 'move example file':
    command => 'mv /home/user/example.txt /home/user/Desktop/'
    onlyif => 'test -e /home/user/example.txt', 
}
```  

### Test and Repair Paradigm    
The actions are taken when only when they are necessary to achieve the goal.  

> Puppet is Stateless  
  
There is no state being kept in seperate run. Each time the puppet agent runs, collects the current facts. The master generates the rules based on the facts.  

### Infrastructure as code
In today's IT the important concept is to treat our Infrastructure as Code. 
This lets us manage of fleet of computers in:  
1. Consistent
2. Versionable
3. Reliable
4. Repeatable   




