---
layout: default
---
# Table Of Contents
* * *

## Please note that Full VM gem5 is not supportive of our meltdown script,
## Since the OS is missing a couple of important kernel files and is missing some commands needed to run our scripts e.g. sudo so you will need to run it on 
## Also note that hydra will not work since python is not available on the machine.

## VM Box or another virtual mahcine locally sorry for the inconvenience!

* [Documentation/Report](https://drive.google.com/open?id=19F1XbiE_Z_MI4YXEhWBpS5tq4TgiVvsF)

* [Preparing Environment](#prep)

* [Spectre Execution](#spectre)

* [Meltdown Execution](#meltdown)

* * *

<a name="prep"/>

# Preparing Environment: #
## Method 1.) Downloading and Using Virtual Box (latest version optional): ##

> Note make sure you have access to su or sudo on your machine some executions may need root privileges.

### Step 1.) Downloading Ubuntu 16.04.1 LTS (Xenial Xerus) Disk Image ###
  
* Download <a href='http://old-releases.ubuntu.com/releases/xenial/ubuntu-16.04.1-desktop-amd64.iso'>Here</a>


### Step 2.) Setup Virtual Box and Mount/Install the Ubuntu Disk And Start VM ###

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
 
 
### Step 4.) Download the source/implementation zip files ###
 

Download gem5 and Spectre Source <a href='https://drive.google.com/open?id=1vMsiGH6DTECydV-p1GsLD_02lr0uW26V'>here</a>

Download Meltdown Source <a href='https://drive.google.com/open?id=1928-VL4fzJrnMsg1sK3QLRS0hH_00Rnw'>here</a>

* Go to the folder you downloaded your zip files

* Extract the zip files:  `unzip gem5.zip` and `unzip meltdown_source.zip`

* * *

<a name='spectre'>

# Executing Spectre (VM Box):

### Run this command below first, this is important for step 2!! ###

1.) 
* cd gem5
* run command: `./build/X86/gem5.opt configs/learning_gem5/part1/two_level.py spectre`


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

* run command: `./build/X86/gem5.opt --debug-flags=O3PipeView --debug-file=pipeview.txt --debug-start=13062347000 configs/learning_gem5/part1/two_level.py spectre`

* Then use `CTRL + C` to stop after the first 2 or 3 letters

3.) 
> Notice in your current directory there is a file called pipeview.txt DO NOT CAT THIS FILE we will need to review this file using > a utility provided by gem5 to "beautify" the text for us.
> 
> We will color code the states in our pipeline using ASCII text


* run command: `util/o3-pipeview.py --store_completions m5out/pipeview.txt --color -w 150`

Once this is completed we will now use this command to see our pipeline: 
* `less -r o3-pipeview.out`

<a name='meltdown'/>

# Executing Meltdown (VM Box):

1.)
Unzip the meltdown_source.zip and cd into meltdown-exploit-master

> Note that the meltdown source files are already made just need to execute the run.sh script

2.)
Run the run.sh script using `./run.sh` and wait for the results.


* * *
