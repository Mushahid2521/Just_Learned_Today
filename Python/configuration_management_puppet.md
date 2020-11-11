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
  

### Permission Mode in Linux  
1. Each file and directory in a Linux system is assigned permission for three Groups of people `the owner, the group and the others`  

2. For each group, the permissions refer to the possibility of reading, writing and executing the file.  

3. It's common to use numbers to represent the permissions: `4 for read`, `2 for write` and `1 for execute`. 
The sum of the permissions given to each of the groups is then a part of the final number. 
For example, a permission of 6 means read and write, a permission of 5 means read and execute, and a permission of 7 means read, write and execute.   

4. XXXX: Special permission, For owner, For group and For others.   

5. For example 0646: The first one represents any special permissions that the file has (no special permissions). 
The second one is the permissions for the owner, (read and write), and then come the permissions for the group (read), 
and finally the permissions for the others (read and write)


### Path Variable   
This is an environment variable that contains an ordered list of paths that Linux will search for executables when running a command.  
Using these paths implies that we don't have to specify the absolute path for each command we want to run.  
The PATH variable typically contains a few different paths, which are separated by colons.  

#### Triggering a manual run of the Puppet agent 
> sudo puppet agent -v --test    
  

>Puppet can be run as Client-Sever architecture as well as Standalone application (useful for testing).  

#### Installing Puppet
>sudo apt install puppet-master  

### Writing a Puppet rule locally
Puppet files which hold the rules are called `Manifest` file.  
Each file end with `.pp` extension.     

```
nano tools.pp
```    
```
package { 'htop':
    ensure => present,
}
```
```
sudo puppet apply -v tools.pp
```  
`-v` stands for verbose to print whats going on.    
  
#### Catalog: 
Catalog are the list of rules that are generated for one specific computer once the server has evaluated all vriables, conditions and functions based on the collected Facts.  

### Managing Puppet Resources
Sometimes some resources are inter-related. Do solve this we need to declare the relationships between them.
Example:

```
class ntp {
    package { 'ntp':
        ensure => latest,
    }
    file { 'etc/ntp.conf':
        source => 'home/user/ntp.conf',
        replace => true,
        require => Package['ntp'],
        notify => Service['ntp'],
    }
    service {'ntp':
        enable => true,
        ensure => running.
        require => File['etc/ntp.conf'],
    }
}

include ntp
```  

Here we are ensuring ntp.conf will be created when the Ntp package will be installed. It also make sure to Notify the service if the conf file changes.  
In the service we are ensure to run it all the time and require the file.  

`include ntp` actually calls the class to run. We will normally include it in another file.  


### Organizing Puppet Modules  
Modules are used to organize the puppet manifest file.  
Module is called a collection of Manifests and associated Data.  

Example: Module for monitoring computers health, network stack, configuring web serving applications.   

A Module consists of mainly 3 directories.  
1. Manifest: Hold all the manifest files.
2. Files: Files that are directly copied to the client machines whithout any changes or irrespective of rules applied. Ex: ntp.conf file.  
3. Templates: The files need to be pre-processed before copying to the client machine. This can be processed based on the facts and rules. 
Templates are documents that combine code, data, and literal text to produce a final rendered output.     
Templates are written in a templating language, which is specialized for generating text from data. Puppet supports two templating languages:
- Embedded Puppet (EPP) 
- Embedded Ruby (ERB) 

Manifest directory must contains a file named `init.pp`. This file must define a class named as the module name.   

By now many pre-packaged modules are available to be directly used by others. They can be installed by:

For downloading Apache module:
```
sudo apt install puppet-module-puppetlabs-apache
```   
These module located in:
```
/usr/share/puppet/modules.available/puppetlabs-apache/
```  

In this directory `lib`(Functions and facts that are already shipped) and `metadata.json` (Additional data about the module).  

To use this Global module we can create a .pp file and include the following:
```
include ::apache
```  
:: indicates it is a Global module.  

#### Puppet Node
We can apply different rules to different computer by using Facts collected or by `Node definition`.  

**Node:** Any system where we can run Puppet Agent. Ex: Server, Computer or even Network router.   

#### Applying rules based on the Agent  
We can define the rules based on the FQDNs(Fully Qualified Domain Names).   
If any node doesn't match with the FQDNs defined in the `site.pp` then it will apply the default node rules.   

```
node default {
    class { 'sudo': }
    class { 'ntp':
        servers => ['ntp1.example.com', 'ntp2.example.com']
    }
}
```  

```
node webserver.example.com {
    class { 'sudo': }
    class { 'ntp':
        servers => ['ntp1.example.com', 'ntp2.example.com']
    }
    class { 'apache': }
}
```  

#### Puppet's Certification   
Agent and Server are managed by certification because.  
1. We don't want to share confidential files.  
2. We are actually configuring what we really want.  

There is a CA(Certification Authority) by default in Puppet or we can use our own CA.  

#### Client-Server Architecture   
Install puppet server to the server using.  
```
sudo apt-get install puppetserver 
```   
For testing lets auto sign the CA.  
```
sudo puppet config --section master set autosign true
```  

Connect to the server and install puppet by `sudo apt install puppet`  

We set the server for the client using `sudo puppet config set server ubuntu.example.com`  

We test run by using `sudo puppet agent -v --test`  

Here there will no rules will be applied until we add rules in site.pp

The puppet node definition is present in root of nodes environment. By default it access the client's Production environment.  
`/etc/puppet/code/environments/production/manifests/site.pp`    

After adding the rule we run again `sudo puppet agent -v --test`.  

We need to enable the way so that the Client gets automatically updated when the rules are changed in the server. To do this.  

```
sudo systemctl enable puppet  // enable the puppet at boot
sudo systemctl start puppet  // start the puppet service.

sudo systemctl status puppet  // check the status  
```    

#### Modifying and Testing manifest  
We can test the rule in a test computer.  
We can use `puppet pasrser vaildate` to check the syntax is correct.  
Use `--noop` without applying any operation. Just print the action.  
We can use `rspec tests` to write code to check the test based on the facts value.    

In an Infrastructure production is the parts where the service is executed and served to its users.  

1. Always roll out to test environment running same way as production.
2. Roll out as batches. One batch a day (known as Canaries) and others next day.  
3. Roll out changes part by part so that issue can be found in a small set of changes.  

[http://puppet-lint.com/](http://puppet-lint.com/)   
[https://rspec-puppet.com/tutorial/](https://rspec-puppet.com/tutorial/)  

#### Two puppet facts  
```
if $facts[os][family] == "Debian"
if $facts[kernel] == "windows"  
```  

![Puppet resource dir](/Images/puppet_file_structure.png)  
![site.pp content](/Images/site_pp.png) 


#### A Puppet Module to Reboot a VM after 30 days running
Create a dir in Module named reboot in modules dir.  
Create manifests dir inside it.  
Create a init.pp file and put the following contents.  
 
```
class reboot {
  if $facts[kernel] == "windows" {
    $cmd = "shutdown /r"
  } elsif $facts[kernel] == "Darwin" {
    $cmd = "shutdown -r now"
  } else {
    $cmd = "reboot"
  }
  if $facts[uptime_days] > 30 {
    exec { 'reboot':
      command => $cmd,
     }
   }
}
```   


## Automation in the Cloud  

Software as a Service(SaaS): Cloud provider delivers an entire application or program to the customer. Ex: Gmail, DropBox  
Platform as a Service(PaaS): Cloud provider provides a Pre-configured platform the the customer. Ex: SQL database as service.  
Infrastructure as a Service(IaaS): Supplies only the bare-bones computing experience. Ex: Google Compute Engine, Microsoft Azure Compute.  

Regions: Zone containing one or multiple datacenters. 

>host the dependencies close the main service or same zone.    

Cloud Capacity measurement examples:
1. Storage
2. QPS
3. Total bandwidth served in an hour.  

*QPS:* Queries per second. Servers answering to the queries per second. This is one measure of Capacity.  


 Types of scaling.  
 1. Horizontal: Adding more nodes. Distribute the services across nodes.   
 2. Vertical: Increasing the capacity of the existing node.  
 
 Automatic scaling is when the Cloud provider use Metrics to upscale and downscale.  
 
 *Containers:* Applications that are packaged with their Configurations and Dependencies. Container runs irrespective of the platform.  
 

### Google Cloud VM  
*Reference Image:* Store the contents of the Machine in a reusable format.  
*Templating:* The process of capturing all of the system configuration to let us create VMs in a repeatable way.  

*Disk Image:* Snapshot of a virtual machine's disk at a given point in time.  

> Create an instance with UI then copy the command to create new VMs with same conf later.

`cat /etc/lsb-release`  This shows the OS info.  

`curl wttr.in`  to access weather report in current location in command line. Curl is used to access a webpage through cmd line.  

#### Running a service in Linux  
A service starts to serve automatically when the linux machine starts.  
For example to run a web service. We need to follow the steps:  

1. Write a script that use web application to serve.  
2. Write a Service script with `.service` extension. Ex: 
```
[Unit]
After=Network.target

[service]
ExecStart = /usr/local/bin/python_script.py 80

[Install]
WnatedBy=default.target
```    
3. Copy the python script to `/usr/local/bin/`.  
4. Copy the service file to `/etc/systemd/system/` where service file located.  
5. `sudo systemctl enable sercie_script.service` to enable the service to run automatically.  

> `ps ax` to get the list of running command. Use `| grep ***` to find desired one.  


#### Templating a customized VM  
A snapshot reflects the contents of a persistent disk in a concrete instant in time.   
An image is the same thing, but includes an operating system and boot loader and can be used to boot an instance.   

We have to use *image* to create a template based on it.  
Image will keep most of the things but removes things that will be different across VMs.  



   

 


 
 
  


