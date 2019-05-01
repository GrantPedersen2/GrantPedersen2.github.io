---
layout: default
---
# Table Of Contents
* * *

* [Preparing Environment](#prep)

* [Spectre Execution](#spectre)

* [Meltdown Execution](#meltdown)

* * *

<a name="prep"/>

# Preparing Environment: #
## Method 1.) Downloading and Using VM Box (latest version optional): ##

> Note make sure you have access to su or sudo on your machine some executions may need root privileges.

### Step 1.) Downloading Ubuntu 16.04.1 LTS (Xenial Xerus) Disk Image ###
  
* Download <a href='http://old-releases.ubuntu.com/releases/xenial/ubuntu-16.04.1-desktop-amd64.iso'>Here</a>


### Step 2.) Setup VM Box and Mount/Install the Ubuntu Disk And Start VM ###

* Additional Resource To Help Install: <a href='https://itsfoss.com/install-linux-in-virtualbox/'>Here</a>
  
  
### Step 3.) Open Terminal and Enter The Commands Below: ###

```
     sudo apt-get install build-essential
     
     sudo apt-get install scons
     
     sudo apt-get install python-dev
     
     sudo apt-get install libprotobuf-dev python-protobuf protobuf-compiler libgoogle-perftools-dev
     
     sudo apt-get install automake
```
     
  You can also try this method too:
  * `sudo apt install build-essential git m4 scons zlib1g zlib1g-dev libprotobuf-dev protobuf-compiler libprotoc-dev libgoogle-perftools-dev python-dev python`
 
 <h4 color ='red'>If you have any trouble running or executing the gem5 look here for troubleshooting the libraries you may need to look <a href='http://learning.gem5.org/book/part1/building.html#requirements-for-gem5'>here</a></h4>
 
 
### Step 4.) Download the source/implementation tar file ###
 
Download <a href=''>here</a>
 
* Extract the tar file:  `tar -zxvf project_implementation.tar.gz`
  
* * *

## Method 2.) Using Hydra Environment: ##


### Step 1.) Log On to Hydra ###
  
  
### Step 2.) Use `WinSCP` To Upload The Source File To Your Directory When Logged On To Hydra ###
  
  
### Step 3.) Extract The Tar ###
* `tar -xJf project_implementation.tar.xz`
  
  
### Step 4.) The Source Files Have The Built X86 ISA For You, So Proceed Below On How To Execute Both Exploits ###

* * *

<a name='spectre'>

# Executing Spectre:

### Run this command below first, this is important for step 2!! ###

1.) 

* run command: `build/X86/gem5.opt configs/learning_gem5/part1/two_level.py spectre`


> For normal spectre execution. After the first 2 or 5 letters that pop on screen, use ctrl+c to stop the process. Once the process stops notice at the bottom it should state the following: 
>
> Exiting @ tick 113568969000 because exiting with last active thread context 
Notice 113568969000 is an example it could be different tick, but we will use this in our example.
>
> Please note this "tick" will be used for us to hook the debugger into our gem5 process.
> 
> WARNING: This will run slow and will write a file to the current path you execute this on.


2.) 

Before running the command, make sure to stop at the first 2 or 3 letters the file can grow very fast be careful with this debugger.

* run command: `-build/X86/gem5.opt --debug-flags=O3PipeView --debug-file=pipeview.txt --debug-start=13062347000 configs/learning_gem5/part1/two_level.py spectre`

* Then use `CTRL + C` to stop after the first 2 or 3 letters

3.) 
> Notice in your current directory there is a file called pipeview.txt DO NOT CAT THIS FILE we will need to review this file using > a utility provided by gem5 to "beautify" the text for us.
> 
> We will color code the states in our pipeline using ASCII text


* run command: `util/o3-pipeview.py --store_completions m5out/pipeview.txt --color -w 150`

Once this is completed we will now use this command to see our pipeline: 
* `less -r o3-pipeview.out`

* * *

<a name='meltdown'/>

# Executing Meltdown:

1.)
We need to startup our Bare Metal VM to run our 64 bit vanilla kernel and OS using this command here:

* `build/X86/gem5.opt configs/example/fs.py --kernel ~/gem5/x86-system/binaries/x86_64-vmlinux-2.6.22.9.smp --disk-image ~/gem5/x86-system/disks/linux-x86.img`

Should look something like this below:
![Results](https://raw.githubusercontent.com/GrantPedersen2/GrantPedersen2.github.io/master/ProjectImplementation/Result.PNG)

2.) 
Now that our VM is running we need to attach a built-in simulated terminal to attach to a TCP port `localhost 3456`
Use the command here:
* `gem5/util/term/m5term localhost 3456`

Should look something like this below:
![Results2](https://raw.githubusercontent.com/GrantPedersen2/GrantPedersen2.github.io/master/ProjectImplementation/Result2.PNG)

3.) 
Now We Wait For `2 Years` **:)** 

4.) 
Should Now See this

![Results3](https://raw.githubusercontent.com/GrantPedersen2/GrantPedersen2.github.io/master/ProjectImplementation/Result3.PNG)

Then use the `clear` and `ls` to see this below:

![Results4](https://raw.githubusercontent.com/GrantPedersen2/GrantPedersen2.github.io/master/ProjectImplementation/Result4.PNG)
