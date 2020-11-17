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

Steps:
1. Stop the VM and go to boot disk.  
2. Create new image.  
3. Create new Instance template.  
4. Select custom created image.  
5. Enable HTTP post.  
6. Now create new Instance using new instance from template option.  

We can create VMs from gcloud command by following,
```
gcloud init
gcloud compute instances create --source-instance-template template_name ws1 ws2 ...  
```   

#### Cloud scale deployments  
Easily and automatically upscale and downscale an infrastructure.  

*Load Balancer:* Ensures that each nodes receives a balanced number of requests.  
  
*Round robin* strategy is when load balancer gives each node one request.  

We need to connect database server to the nodes as they are added and removed. This is also served with load balancer but typically managed by 
provider. Database server are used as Platform as a service.   

How web application works in practice with lot of users:
1. IP address has different entry points to enter, if one fails other may work, or to provide good service closer to the zone, load and many others.  
2. For small application this entry points could be the web server that serves the web pages and thats it.  
3. For larger application there are different layers in between the entry point and actual web service.  
4. First layer is Web Caching layer and a load balancer to distribute the load. If the response already in the cache 
it send the response. Ex: Varnish and Nginx. If the response it not available it make request to the actual web server and store the content for future use.   
5.  This layer also contains a load balancer and a pool of actual web server which generate response. It connects with a database server. 
As there are lots of server to make query from DB, there is also a load balancer caching layer. 
The popular application for this level of caching is: Memcached and Redis.   

#### Orchestration  
The automated configuration and coordination of complex IT systems and services.  

To handle the infrastructure as  code, different cloud providers provide different tools for managing the resources as code.  
But to make a common tool for all providers there is *Terraform*. 

Terraform uses domain specific language to interact with different types of cloud provider.  

#### Managing Cloud Storage  
**Storing Data**  
*Block storage* is like a physical hard disk. It can be persistent (permanent) and Ephemeral (temporary) which needs only when the service run.  
Disks of different instances can be connected to the same file system by using Platform as a service with NFS or CIFS protocol.  

*Object Storage/Blob storage* lets place and retrieve data in a storage bucket. The objects can be photos, videos. 
The different types of objects are called blobs. These are accessed through APIs provided by CLoud provider and 
offered as Databases as a service. This db can be SQL and NoSQL.  
Cloud providers offer Database as service with different storage class options based on Throughput, IOops and Latency.  
Hot storage and Cold storage. Hot is when the data is accessed more often and use SSD.  


#### Load Balancing 
More than one machine for same service.  
Round-Robin method converts a url into the list of all available instances ip address and distribute one after another.  
But its not the right way, We can use a load balancing server and use health checks, auto-scaling and more to it while 
distributing load. For tracking a user session we tell the load balancer to use sticky session. This makes route to the same 
machine for same user session.  

CDN(Content Delivery Network) is a service which tries to make the service as close to the user. This cache the requests for first time and when 
any other user make the same request it delivers it fast.    


#### Monitoring and Alerting  
Different metrics are used to monitor the service condition. Some metrics are generic and some are specific to the service.   
For error code in 404 range means client side problem.  
Error code in range 500 means internal server problem,  

All the cloud providers have their own monitoring system. There are some other systems whih might be used cross providers.  

The metrics are collected using Pull(Make query to service to get the metrics at interval) and Push(Periodically push the metrics to the Monitoring System).  

The main theme behind Monotoring and Alerting is that we Periodically check the state of the system and raise alert when something wrong.  

We need to divide alerts into Urgent and Non-Urgent groups or bugs. Urgent alerts are called Pages, pages are sent with sms, email or audio call.     

**Service Level Goals(SLO):** Pre-established performance goal for a specific service. This should be mesaurable and check if its meeting the objectives.    

Availability of a service is measure using number of `9`s. Five 9 service means 99.999% availability which requires a larger team to manage this availability. Usually 3-9 or 2-9 is pretty good service.    

**Service Level Commitment:** Commutment between a client and a provider.   

#### Systemctl in Linux
Systemctl is a utility for controlling the systemd system and service manager. It comes with a long list of options for different functionality, including starting, stopping, restarting, or reloading a daemon.    

#### Troubleshooting a 5000 error   
Check the status of the apache server using.   
```
sudo systemctl status apache2
```   

if it shows error. Restart it using   
```
sudo systemctl restart apache2
```   

If it fails again check the port is being used by the server.   
```
netstat -nlp
```  

If we find one process using port 80, then we are getting the erro for this.  
To get the list of all processes running and isolating, we use  
```
ps -ax | grep python3 
```   

Kill the process using default port 80 using.  
```
sudo kill pid_
```  

This may start the service with different PID. To kill and disable the service we will first find the service and disable it.   
```
sudo systemctl --type==service | grep service_name    
sudo systemctl stop service_name && sudo systemctl disable service_name  
```  

We restart the Apache.  
```
sudo systemctl start apache2
```  





    
   



   

 


 
 
  


