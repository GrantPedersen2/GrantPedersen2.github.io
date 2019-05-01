# Preparing Environment: #
## Method 1.) Downloading and Using VM Box (latest version optional): ##

### Step 1.) Downloading Ubuntu 16.04.1 LTS (Xenial Xerus) Disk Image ###
  
* Download <a href='http://old-releases.ubuntu.com/releases/xenial/ubuntu-16.04.1-desktop-amd64.iso'>Here</a>

### Step 2.) Setup VM Box and Mount/Install the Ubuntu Disk And Start VM ###

* Additional Resource To Help Install: <a href='https://itsfoss.com/install-linux-in-virtualbox/'>Here</a>
  
### Step 3.) Open Terminal and Enter The Commands Below: ###
  
     sudo apt-get install build-essential
     
     sudo apt-get install scons
     
     sudo apt-get install python-dev
     
     sudo apt-get install libprotobuf-dev python-protobuf protobuf-compiler libgoogle-perftools-dev
     
     sudo apt-get install automake
     
  You can also try this method too:
  * sudo apt install build-essential git m4 scons zlib1g zlib1g-dev libprotobuf-dev protobuf-compiler libprotoc-dev libgoogle-perftools-dev python-dev python
 
 <h4 color ='red'>If you have any trouble running or executing the gem5 look here for troubleshooting the libraries you may need to look <a href='http://learning.gem5.org/book/part1/building.html#requirements-for-gem5'>here</a></h4>
 
### Step 4.) Download the source/implementation tar file ###
 
Download <a href=''>here</a>
 
* Extract the tar file:  tar -zxvf project_implementation.tar.gz
  
<h2 color ='red'>Note make sure you have access to su or sudo on your machine some executions may need root privileges.</h2>

## Method 2.) Using Hydra Environment: ##

### Step 1.) Log On to Hydra ###
  
### Step 2.) Use WinSCP To Upload The Source File To Your Directory When Logged On To Hydra ###
  
### Step 3.) Unzip The Tar ###
* tar -zxvf project_implementation.tar.gz
  
### Step 4.) The Source Files Have The Built X86 ISA For You, So Proceed Below On How to Execute Both Exploits ###

## Executing Spectre: ##

### First run the command below this important for step 2 ###

1.) 

* run command: build/X86/gem5.opt configs/learning_gem5/part1/two_level.py spectre

<h4 color ='red'> 
For normal spectre execution. After the first 2 or 5 letters that pop on screen, use ctrl+c to stop the process. Once the process stops notice at the bottom it should state the following: 

<b>
Exiting @ tick 113568969000 because exiting with last active thread context 
Notice 113568969000 is an example it could be different tick, but we will use this in our example.
  
</b>

Please note this "tick" will be used for us to hook the debugger into our gem5 process.
</h4>

<h4 color ='red'>
WARNING: This will run slow and will write a file to the current path you execute this on.
</h4>

2.) Before running the command, make sure to stop at the first 2 or 3 letters the file can grow very fast be careful with this debugger.

* run command: -build/X86/gem5.opt --debug-flags=O3PipeView --debug-file=pipeview.txt --debug-start=13062347000 configs/learning_gem5/part1/two_level.py spectre

* Then use CTRL + C to stop after the first 2 or 3 letters


<h3 color ='red'>3.) Notice in your current directory there is a file called pipeview.txt DO NOT CAT THIS FILE we will need to review this file using a utility provided by gem5 to "beautify" the text for us.

We will color code the states in our pipeline using ASCII text
</h3>

* run command: util/o3-pipeview.py --store_completions m5out/pipeview.txt --color -w 150

Once this is completed we will now use this command to see our pipeline: 
* less -r o3-pipeview.out

### Executing Meltdown: ###
