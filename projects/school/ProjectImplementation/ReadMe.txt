## Preparing the environment:

1.) Download the source <a>here</a>
  
2.) Install packages for gem5 running this <a>script</a> 

3.) Note make sure you have access to su or sudo on your machine
some executions may need root privileges.

## Execute Spectre:
First run the command below this important for step 2
1.) run command: <b>build/X86/gem5.opt configs/learning_gem5/part1/two_level.py spectre</b>

For normal spectre execution. After the first 2 or 5 letters that pop on screen, use ctrl+c to stop the process.
Once the process stops notice at the bottom it should state the following:
<b>Exiting @ tick 113568969000 because exiting with last active thread context</b>
Notice 113568969000 is an example it could be different tick, but we will use this in our example

Please note this "tick" will be used for us to hook the debugger into our gem5 process.

2.) <b>WARNING: This will run slow and will write a file to the current path you execute this on.</b>

Before running the command, make sure to stop at the first 2 or 3 letters 
the file can grow very fast be careful with this debugger.

run command: 
-<b>build/X86/gem5.opt --debug-flags=O3PipeView --debug-file=pipeview.txt --debug-start=13062347000 configs/learning_gem5/part1/two_level.py spectre </b>
-<b>CTRL + C</b> to stop

3.) Notice in your current directory there is a file called pipeview.txt
<b> DO NOT CAT THIS FILE </b> we will need to review this file using
a utility provided by gem5 to "beautify" the text for us.

We will color code the states in our pipeline using ASCII text

run command:
<b> util/o3-pipeview.py --store_completions m5out/pipeview.txt --color -w 150 </b>

Once this is completed we will now use this command to see our pipeline:
<b> less -r o3-pipeview.out </b>

## Execute Meltdown:
