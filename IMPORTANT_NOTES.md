
# IMPORTANT_NOTES
These are very important things that I have learned while programming.
Some of the things listed here are just some useful commands incase
that I forget them.

### TO WRITE CODE
<pre><code>yes | script </code></pre>
versus

<code>yes | script </code>

##### You can also do this with ticks from the ~ key.
Here is a `single` piece of code.
```
and here is a block of code.
```

### ADD TO PATH
- `echo $PATH` **This will show what is currently in your path**
- `env | grep PATH`**This does the same thing**
- `export PATH=/home/pi/:$PATH` **Put this in .bashrc to add to path**
- `source ~/.bashrc` **This activates the changes made so that a new terminal does not need to be opened**

### TO FIND A FILE
	# Use find function
		find / -name \*.jpg			# finds all jpgs anywhere on the system
		find /home -name \*.jpg		# finds all jpgs anywhere in home directory and below
		find /home -iname \*.jpg	   # finds all jpgs anywhere in home directory regardless of capitalization
		find / -mtime +6			   # finds all files modified MORE than 6 days ago
		find / -mtime -2	           # finds all files modified LESS that 2 days ago
	# Use locate (once installed)
		apt-get install mlocate	    # installs software
		updatedb					   # need to rund this everytime to update software
		locate myfile.jpg			  # finds location of file

### MULTIPLE COMMANDS
	# Command2 will be executed if (and only if) command 1 returns exit status zero
		command1 && command2
	# Both command1 and command2 will be executed regardless
		command1 ; command2

### PRINT OUTPUT TO A FILE
	sudo apt-get update > out.txt && echo All Finished
	# Instead of the output being sent to the terminal, it will be written to the text file
	# and then it will say "All Finished" in the command line to let you know that it is finished








end
---
