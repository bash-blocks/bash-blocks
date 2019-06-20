# bash-blocks

## Version

**The latest stable version is:**
```python
+ 0.6.1-beta
```    
The master branch is in sync with the latest stable version.     

## About bash-blocks

**bb** is a single text framework for the rapid application development of any scriptable project over one or more machines.   
It is written in bash because sometimes thats all you can use in locked down environments (banking).  
Influenced by javascript frameworks, **bb** has the portability of **JQuery** combined with the extensibility of **NodeJs**.       
* it is a single minimised bash file that may be copy and pasted between terminal windows.  
* modules of functionality (**blocks**) are easily installed with either the **bb** package manager or copy and pasting.    
* it does not require elevated permissions to be run.      
* via json the end-users have a simple declarative approach to defining server and meta data for a given **block**.   
* it is developer friendly and offers rapid application development for any scriptable task to become a **block**.   
* it is tested on Mac, Ubuntu, Centos and Redhat.  
* it has no installed dependencies requiring only a bash version greater than 4.0 (now standard across enterprise).**<sup>1</sup>**         

**<sup>note 1</sup>**     
--> *The json parsing library **jq** is provided and like **bb** can be copy and pasted between terminal windows.*  
--> *On a Mac (i.e. development laptops) elevated user rights and the homebrew package manager are required.*  

### Stages, tasks and actions

**bb** offers a generic workflow for any project.  
This workflow consists of multiple **stages**, each with one or more **tasks** consisting of one or more **actions**.  

**Stage 1:** do something on the local machine.**<sup>2</sup>**   
**Stage 2:** test connectivity to the server(s) defined in the json defintion file.  
**Stage 3:** test required write path permissions for **bb** and **block** on each server.  
**Stage 4:** copy **bb** and **block** code to each server.  
**Stage 5:** copy any **block** specified files to each server.  
**Stage 6:** execute **block** code on each remote server.   
**Stage 7:** finish with **stage** report and any **block** specific messages.      

Depending on the **block** being run (i.e. dictated by its developer), either all or a subset of these **stages** may be run.  
* **Stages 1 and 7** only execute on the machine running **bb**.  
* **Stages 2 to 6** are performed asynchronously against each remote server in parallel.

**<sup>note 2</sup>**     
--> *This optional stage could utilise the pre-built 'make build folder' stage and / or utilise a **block** specific set of **tasks***.    
--> *The 'make build folder' stage provides a way of creating a versioned folder of resources for a json definition file.*   
--> *See **block-dse-6.7.x** as an example of how this helps to version segregate different settings for a dse deployment.*      
----> https://github.com/bash-blocks/block-dse-6.7.x

### Block development

* **Block developers** can enjoy a reliable and rapid development experience.  
* Quick integration with stages 2-5 and 7 - requires only filling in predefined arrays.  
* Creating **tasks** for **stages** 1 and 6 is accelerated via a library of **bb** functions.      
* A helper script minimises the developer's code into a single **block** file.   
* For more guidance in developing a bash-block - see this guide (link coming soon!).  

## Quick start

### Install bb

The git clone and download zip (from github) commands will create a 'bash-blocks' parent folder.    
If copy and pasting files, first create this parent folder.   


```bash
$ git clone https://github.com/bash-blocks.git # or download zip or copy and paste bb + jq files
$ chmod +x bb                                  # make bb executable
$ ./bb --help                                  # display the help screen
$ ./bb --install-block bb                      # install bb locally and follow on-screen instructions
```

The install will:  
1. Create a local folder structure in the same directory as **bb**.    
```
--bash-blocks    
----bb
----jq
----blocks
----installed-blocks
----servers_json_connect
```
2. Add an entry to the bash_profile (+ bashrc on Ubuntu systems), so **bb** can be run from any folder.  

![alt text](https://github.com/jondowson/bash-blocks-logo/blob/master/Screenshot%202019-06-19%20at%2022.06.14.png?raw=true ".bash_profile")

3. On a Mac install/refresh 'homebrew' package manager and retrieve a number of packages.    

### Password-less server access           

**bb** requires ssh password-less connection to be setup for the local machine as well as those servers defined in each json definition file.     

```bash
$ ssh-keygen -t rsa                # do this if a local key does not already exist + hit enter to all questions.    
$ ssh-copy-id user@127.0.0.1       # copy the public key to the local server.       
$ ssh-copy-id user@remote-machine  # copy the public key to any remote server specified in a json definition.
```
Alternatively use this command if ssh-copy-id is not available.   
```bash
cat ~/.ssh/id_rsa.pub | ssh <user>@<hostname> 'umask 0077; mkdir -p .ssh; cat >> .ssh/authorized_keys && echo "Key copied"'
```
### Install a block

Once you have sourced your bash profile to apply the path settings, you can run bb directly from any folder.

```bash
$ bb --install-block block-abc   # install this block from the bash-blocks github repo.
```

Alternatively download (or copy and paste) the block from another destination and add it to the **blocks** subfolder.    
Then run the above --install-block command.  

## Help

The help screen lists all the flags the **bb** framework.

```bash
$ bb --help
```

![alt text](https://github.com/jondowson/bash-blocks-logo/blob/master/Screenshot%202019-06-19%20at%2020.08.56.png?raw=true "bb --help")

## Example json definition

* Each block makes use of a json defintion file to define server connection settings and meta-data.   
* Multiple json definitions can quickly be created for different environments.     
* End users define build settings across a cluster, whilst server settings can be unique to that machine.   
* All other settings not included in the json can still be edited by using the 'make build folder' stage.   
--> this for example can retain copies of all config files in a build folder (named the same as the json defintion).   
--> edits to these files can be applied across all servers. i.e. only settings that require server level edits need be added to the json definition.        

* For **block** developers, **bb** will handle any well formed json and it will auto convert all json properties to easily accessible bash variables.   
--> e.g. from below ${servers_server1_dse_mode_graph} is equal to 'false'

![alt text](https://github.com/jondowson/bash-blocks-logo/blob/master/Screenshot%202019-06-19%20at%2020.24.05.png?raw=true "example json")

## Example error output

If user enters wrong flags or for debugging during the development of a block, stack trace messages are generated.    

![alt text](https://github.com/jondowson/bash-blocks-logo/blob/master/Screenshot%202019-06-19%20at%2022.12.57.png?raw=true "bb stack trace")
