---
layout: default
---
# Table Of Contents
* * *

> Please note that Full VM gem5 is not supportive of our meltdown script,
> Since the OS is missing a couple of important kernel files and is missing some commands needed to run our scripts e.g. sudo so you will need to run it on Virtual Box or another virtual mahcine locally sorry for the inconvenience!
> Also note that hydra will not work since python is not available on the machine.

* [Documentation/Report At End Of Report Shows Proof Of Correctness](https://drive.google.com/open?id=19F1XbiE_Z_MI4YXEhWBpS5tq4TgiVvsF) 

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

* NOTE YOU MUST HAVE AT LEAST 20 - 25 GB OF MEMORY FOR YOUR HARD DRIVE!!!
  
### Step 3.) Open Terminal and Enter The Commands Below: ###

```
     sudo apt-get update
     
     sudo apt-get install git
     
     sudo apt-get install build-essential
     
     sudo apt-get install scons
     
     sudo apt-get install python-dev
     
     sudo apt-get install libprotobuf-dev python-protobuf protobuf-compiler libgoogle-perftools-dev
     
     sudo apt-get install automake
     
     sudo apt-get install libboost-all-dev
     
     sudo apt-get install pip (if this does not work try): sudo apt-get install python-pip
     
     sudo pip install six
```
     
  You also need to run this method as well:
  * `sudo apt install build-essential git m4 scons zlib1g zlib1g-dev libprotobuf-dev protobuf-compiler libprotoc-dev libgoogle-perftools-dev python-dev python`
 
 In a last ditch effort if it is not working try this command below and rerun all commands above:
 * `sudo apt-get upgrade libprotobuf-dev`
 
 <h4 color ='red'>If you have any trouble running or executing the gem5 look here for troubleshooting the libraries you may need to look <a href='http://learning.gem5.org/book/part1/building.html#requirements-for-gem5'>here</a></h4>
 
 You can also see this for your PIP issue: <a href='https://askubuntu.com/questions/672808/sudo-apt-get-install-python-pip-is-failing'> here</a>
 
 
### Step 4.) Download the source/implementation zip files ###

Download the git source of gem5:

* `cd /home/your_username/Documents/`

* `git clone https://gem5.googlesource.com/public/gem5`

Now open the FireFox Browser in your Ubuntu and use the links below

Download CPU File <a href='https://drive.google.com/open?id=1XlYqC1fD-D27Y4DHkfmDlDV1BY_WVPPA'>here</a>

Download Spectre Execution <a href='https://drive.google.com/open?id=11w6ncM0T529_sqTKVnOgRlNKg8kyPjnk'>here</a>

Download Spectre Source <a href='https://drive.google.com/open?id=1tVA02jRx1YYNXDcUEZxwLH0TelHVL7G_'>here</a>

Download Meltdown Source <a href='https://drive.google.com/open?id=1928-VL4fzJrnMsg1sK3QLRS0hH_00Rnw'>here</a>

* Go to the folder you downloaded your zip files using `cd` or you can use the GUI provided by Ubuntu

* Extract the zip files: `unzip spectre_source.zip` and `unzip meltdown_source.zip` or use the GUI

* run command `mv /home/your_username/Downloads/two_level.py /home/your_username/Documents/gem5/configs/learning_gem5/part1/two_level.py`

* run command `mv /home/your_username/Downloads/spectre /home/your_username/Documents/gem5/`

### Step 5.) Build the X86 ISA For gem5

* First Run: `cd /home/your_username/Documents/gem5`

PLEASE NOTE -j takes your number of cores + 1
E.G. I have 1 core so 1+1 = 2 Cores to run, this is needed!

* Then Build: `scons build/X86/gem5.opt -j2`

* * *

<a name='spectre'>

# Executing Spectre (VM Box):

### Run this command below first, this is important for step 2!! ###

1.) 
* run command: `cd /home/your_username/Documents/gem5`

* run command: `./build/X86/gem5.opt configs/learning_gem5/part1/two_level.py spectre`


> For normal spectre execution. After the first 2 or 5 letters that pop on screen, use ctrl+c to stop the process. Once the process stops notice at the bottom it should state the following: 
>
> Exiting @ tick 113568969000 because exiting with last active thread context 
Notice 113568969000 is an example it could be different tick, but we will use this in our example.
>
> Please note this "tick" will be used for us to hook the debugger into our gem5 process.
> 
> WARNING: This will run slow and will write a debug file to the current path you execute this on.


2.) 

Before running the command, make sure to stop at the first 2 or 3 letters the file can grow very fast be careful with this debugger.

* run command: `./build/X86/gem5.opt --debug-flags=O3PipeView --debug-file=pipeview.txt --debug-start=13062347000 configs/learning_gem5/part1/two_level.py spectre`

* Then use `CTRL + C` to stop after the first 2 or 3 letters

3.) Proof Of Correctness
> Notice in your current directory there is a file called pipeview.txt DO NOT CAT THIS FILE we will need to review this file using > a utility provided by gem5 to "beautify" the text for us.
> 
> We will color code the states in our pipeline using ASCII text

* run command: `util/o3-pipeview.py --store_completions m5out/pipeview.txt --color -w 150`

Once this is completed we will now use this command to see our pipeline: 
* `less -r o3-pipeview.out`

You can check the pipeline for correctness of Spectre
First look at the assembly version of our spectre when compiled:

```

# NOTE: the movzbl below is MOVZX_B_R_M in gem5.
# it is implemented with the following microcode.
#    ld t1, seg, sib, disp, dataSize=1
#    zexti reg, t1, 7
#
000000000040105e <victim_function>:
  40105e:   55                      push   %rbp
  40105f:   48 89 e5                mov    %rsp,%rbp
  401062:   48 89 7d f8             mov    %rdi,-0x8(%rbp)
  401066:   8b 05 14 f0 2b 00       mov    0x2bf014(%rip),%eax # 6c0080 <array1_size> load array1_size (first time is always a miss)
  40106c:   89 c0                   mov    %eax,%eax
  40106e:   48 3b 45 f8             cmp    -0x8(%rbp),%rax  # if (x < array1_size) rax is array1_size, -8(%rbp) is x
  401072:   76 2b                   jbe    40109f <victim_function+0x41> # if (x < array1_size)
  401074:   48 8b 45 f8             mov    -0x8(%rbp),%rax # load x from the stack into rax
  401078:   48 05 a0 00 6c 00       add    $0x6c00a0,%rax  # calculate array1 offset (x+array1)
  40107e:   0f b6 00                movzbl (%rax),%eax # load array1[x]
  401081:   0f b6 c0                movzbl %al,%eax    # zero extend to 32 bits
  401084:   c1 e0 09                shl    $0x9,%eax   # multiply by 512
  401087:   48 98                   cltq               # sign-extend eax
  401089:   0f b6 90 80 1d 6c 00    movzbl 0x6c1d80(%rax),%edx  # load array2[array1[x]*512] **** This is the magic!
  401090:   0f b6 05 e9 0c 2e 00    movzbl 0x2e0ce9(%rip),%eax        # 6e1d80 <temp> Load temp.
  401097:   21 d0                   and    %edx,%eax
  401099:   88 05 e1 0c 2e 00       mov    %al,0x2e0ce1(%rip)        # 6e1d80 <temp>
  40109f:   5d                      pop    %rbp
  4010a0:   c3                      retq
  
```

> we want to find a time where the `movzbl` at address `0x401089` is executed speculatively and to see the time where the load completes for the instruction at 0x401089

> From the generated image, the instruction at 0x401066 causes a cache miss (there is a long time between when the load is issued and the data is returned from memory). Since the load of array1_size was a cache miss, the jump at 0x401072 is speculated to be not taken (incorrectly). This causes the following instructions to be executed speculatively, eventually squashed in the pipeline.

> The key thing in this trace that is the Spectre vulnerability is that the load for the instruction at 0x40107e, which loads secret data happens during the mis-speculated instructions. Then, the data is loaded into the registers and operated on instruction 0x401084. Finally, the load at address 0x401089 is executed and loads the value from memory that is dependent on the secret data loaded previously. Thus, we can later probe the cache to retrieve the secret data.

<a name='meltdown'/>

# Executing Meltdown (VM Box):

1.)
Unzip the meltdown_source.zip and cd into meltdown-exploit-master

> Note that the meltdown source files are already made just need to execute the run.sh script

2.)
Run the run.sh script using `./run.sh` and wait for the results.

## Proof Of Correctness:

Remember we are obtaining the string `version` from the private variable static char expected[] = "%s version %s"; which is known in and accessed by the kernel memory.

Note use of the `/proc/kallsyms` shows proof of correctness to execute on kernel addresses that are displayed in the 
output of our program.  Kernel Symbols are global, represent space in the memory, provides a symbol for the name of the variable and are used as debugging variables that we can utilize for our meltdown.  We couple this program with `/boot/System.map` which is used as a symbol table by the kernel itself in our run.sh.  Both `System.map` and `kallsyms` allows us to debug the kernel variables and print them to stdout and ARE REQUIRED TO RUN THE MELTDOWN PROGRAM.

We then use a "training" system similar to the spectre where we need to account for accuracy in retreiving the kernel addresses in byte form with the help of `kallsyms` and `System.map`; remember we can have access to kallsyms for debugging perpuroses to know exactly what the byte representation and hex value of a particular kernal variable address.

Below is the snippit of code to help score the expected byte return after performing a meltdown attack
Then we print out that hex address and value to the user and the score of how exact the byte form was expected.
```

  for (score = 0, i = 0; i < size; i++) {
		ret = readbyte(fd, addr);
		if (ret == -1)
			ret = 0xff;
		printf("read %lx = %x %c (score=%d/%d)\n",
		       addr, ret, isprint(ret) ? ret : ' ',
		       ret != 0xff ? hist[ret] : 0,
		       CYCLES);

		if (i < sizeof(expected) &&
		    ret == expected[i])
			score++;

		addr++;
	}

	close(fd);

	is_vulnerable = score > min(size, sizeof(expected)) / 2;
  
```

* * *
