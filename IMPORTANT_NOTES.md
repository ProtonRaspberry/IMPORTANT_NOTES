
# IMPORTANT_NOTES
These are very important things that I have learned while programming.
Some of the things listed here are just some useful commands incase
that I forget them.

### Atom (install this text editor)
Atom has a great markdown preview option so that you can see how it will look before committing the file to github.

Also, go to `packages` at the top Atom toolbar, then `setting view`, then `Install Packages/Themes`.  Search for `markdown-scroll-sync` by [mark-hahn](https://atom.io/packages/markdown-scroll-sync).  It is awesome and will automatically scroll while you edit.

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
---
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
---
### MULTIPLE COMMANDS
	# Command2 will be executed if (and only if) command 1 returns exit status zero
		command1 && command2
	# Both command1 and command2 will be executed regardless
		command1 ; command2
---
### PRINT OUTPUT TO A FILE
	sudo apt-get update > out.txt && echo All Finished
	# Instead of the output being sent to the terminal, it will be written to the text file
	# and then it will say "All Finished" in the command line to let you know that it is finished
---
### SEND EMAILS AND SMS TEXT MESSAGES
##### For email:

I got this tutorial from [raspberry-projects.com](http://www.raspberry-projects.com/pi/software_utilities/email/ssmtp-to-send-emails)

Another great [resource](http://iqjar.com/jar/sending-emails-from-the-raspberry-pi/)

```bash
sudo apt-get install ssmtp
sudo apt-get install mailutils
sudo nano /etc/ssmtp/ssmtp.conf
```
(include in this file)
```bash
root=postmaster
mailhub=smtp.gmail.com:587
hostname=raspberrypi
AuthUser=AGmailUserName@gmail.com
AuthPass=TheGmailPassword
FromLineOverride=YES
UseSTARTTLS=YES
```
#### To send an email type:
```bash
echo "Hello world email body" | mail -s "Test Subject" recipientname@domain.com
```
#### To send a file
```bash
sudo apt-get install mpack
mpack -s "Test" /home/pi/some_folder/somefile.ext recipientname@domain.com
```

#### For SMS:

I found this information from [raspberrypi.org](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=69286)

There is a tutorial there for another method, or you could just do this (it worked for me)

```
Try sms via email
here is a list by provider
Provider Format
Sprint phonenumber@messaging.sprintpcs.com
Verizon phonenumber@vtext.com
T-Mobile phonenumber@tmomail.net
AT&T phonenumber@txt.att.net
AIM +1phonenumber
```
---
### Samba File Server

```bash
sudo apt-get update
sudo apt-get install samba samba-common-bin
```
Now to edit the `.conf` file:

`sudo nano /etc/samba/smb.conf` and edit/add these lines to the file:

```
workgroup = WORKGROUP
wins support = yes

[pi]
	comment = Pi Server
	path = /
	browseable = yes
	writeable = yes
	only guest = no
	create mask = 0777
	directory mask = 0777
	public = no

	force user = root
```
###### NOTE: that what ever is in those brackets must be the path in the computer.  By default, this is "pi".  If something else is written there, it will not work.

Exit the `.conf` file and type this into the terminal:

```bash
sudo smbpasswd -a pi          # (enter password and confirm)
sudo smbpasswd -a root        # (enter password and confirm)
sudo service smbd restart     # restarts the samba service
```
You may use `testparm` in the terminal to test the `.conf` file for syntax errors. (This is a built-in command when you installed samba)

You will now see **RASPBERRYPI** under **network** in the file explorer

Three references were used to do this:
- [one](http://raspberrywebserver.com/serveradmin/share-your-raspberry-pis-files-and-folders-across-a-network.html)
- [two](https://www.theurbanpenguin.com/setting-up-a-samba-server-on-raspberry-pi/)
- [three](https://askubuntu.com/questions/754572/cannot-restart-samba-samba-service-is-masked)

---
### Python (The Basics)
The following needs to be included at the beginning of python scripts:
```python
#!/usr/bin/python
import RPi.GPIO as GPIO		# imports the Pi's GPIO
import time					# imports time (for sleep functions, etc)
import spidev                  # imports the SPI interface for analogue inputs

spi = spidev.SpiDev()
spi.open(0,0)

# Now you should define all functions you may have
def MyFunction(InputParameter)
    try:
			while True:
				if GPIO.input(17):
					print("LED off")
				else:
					pass
				time.sleep(0.1)
# Now that functions are all finished, set up the GPIO:
GPIO.setmode(GPIO.BCM)			    # or set to GPIO.BOARD for board numbering system
GPIO.setup(PinNumber, GPIO.IN)		# sets a particular pin as input
GPIO.setup(PinNumber, GPIO.OUT)	   # sets a particular pin as output

GPIO.output(PinNumber, GPIO.LOW)	  # initializes the pin to low, 0.0[V]
GPIO.output(PinNumber, GPIO.HIGH)	 # initializes the pin to high, +3.3[V]

try:
	while True:
		if Something:
			print("SomeNonsense")
		else:
			pass
		time.sleep(0.1)

# Exception so that Ctrl+C can interrupt our program
except KeyboardInterrupt:
	print("\n Ctrl+C has been pressed!")

# No matter how this program is terminated, this block of code will always run
# It is good practice to cleanup the GPIO.  Put this at the end of every single
# program that you write.
finally:
	spi.close()
	GPIO.cleanup()
```
**Remember that bash and python boolean data is different**

for bash:
- `True = 0`
- `False = 1`

for python:
- `True = 1`
- `False = 0`
---

### Soil Moisture Sensor





### Start Program on Boot

- `sudo nano /etc/rc.local` This lets you put the command to start a program on boot.  There are several ways to do it - this is just one of them.  Add the command to the end of the file:

- `sudo python NameOfYourProgram.py &` The `&` is very important or else the computer will not boot.

- For the washing Machine, I have `sudo /home/pi/MY_SCRIPTS/WashingMacineBash &`, which then exectutes a bash script.  This bash script, then executes a the washing machine python script (which is an infinite loop).

- Once the final shutdown sequence is initiated, the python script will exit and go back to the bash script where it left off.  The bash script gives the command for actual shutdown `sudo shutdown -h now`

### NewTitleHere
### NewTitleHere
### NewTitleHere

---
> This is pretty neat!

EOF (end of file)
---
