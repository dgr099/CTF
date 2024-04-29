


# Anonymous penguin

<span class="font-0">As always, the first steps will involve preparing the lab and performing a quick exploration using nmap to check which ports are open. In this case, ports 80 and 21 are open.</span><br>

![Image](images/output-1_1.png)

<span class="font-0">Next, we'll attempt to access the web in search of anything of interest.</span><br>

![Image](images/output-1_2.png)

<span class="font-0">Unfortunately, the website doesn't offer much to extract, so let's try conducting directory enumeration with gobuster to gather more information.</span><br>
![Image](images/output-2_1.png)



<span class="font-0">One of the results is a directory named "upload," which might allow us to upload files or access others from there, potentially enabling a reverse shell.</span><br>
<span class="font-0">For now, let's return to the FTP on port 21 and attempt to access it anonymously.</span><br>

![Image](images/output-2_2.png)

<span class="font-0">One of the directories in the FTP is named "upload," just like what we found with gobuster.</span><br>
<span class="font-0">Let's try sending a simple PHP file to verify if we can access it.</span><br>



![Image](images/output-3_1.png)

<span class="font-0">Indeed, the name is displayed on the screen, indicating that we're likely able to establish a reverse shell. We can use the web shell generator to deploy any of its scripts.</span><br>

![Image](images/output-3_2.png)

<span class="font-0">We upload the file and successfully establish a connection, obtaining our reverse shell.</span><br>
<span class="font-0">The user we access with is www-data, which can execute the command /usr/bin/man as the user pingu without requiring a password (NOPASSWD).</span><br>

![Image](images/output-4_1.png)


<span class="font-0">The issue is that to escalate privileges, we need an interactive shell that allows us to abort and return to the shell with penguin's permissions.</span><br>

![Image](images/output-4_2.png)

<span class="font-0">We tried several methods, but we can't even install socat, the commands seem not to work, and the Python console doesn't allow us to interrupt the man to return to the shell.</span><br>

![Image](images/output-5_1.png)

![Image](images/output-5_2.png)

![Image](images/output-5_3.png)

<span class="font-0">I was about to give up when I finally managed to get the interactive shell.</span><br>
<span class="font-0">First, we'll execute the command script /dev/null -c bash to start a new bash session and save the output to /dev/null.</span><br>
<span class="font-0">Then, we suspend the session with Ctrl + Z and subsequently set the tty to stty raw -echo to transmit characters directly without processing and without echo.</span><br>
<span class="font-0">We resume the suspended session with fg. Next, we reset the terminal with reset xterm to ensure it's in a clean and usable state.</span><br>

<span class="font-0">Subsequently, we adjust the terminal size with stty rows 62 columns 248 to match the dimensions of our machine's screen, and finally, we set the environment variables TERM and SHELL with export TERM=xterm and export SHELL=bash, respectively, to ensure proper interaction with the terminal.</span><br>
<span class="font-0">With this setup, we can finally execute the privilege escalation.</span><br>

![Image](images/output-5_4.png)

We perform the privilege escalation obtained from gtfobins and finally we manage to gain access as pingu.

![Image](images/output-6_1.png)


Its time to  check the response provided by sudo -l for pingu so we can check his sudo privileges.

![Image](images/output-6_2.png)

<span class="font-0">None of the options for privilege escalation work correctly with nmap, so we try with dpkg.</span><br>

![Image](images/output-6_3.png)

<span class="font-0">With dpkg, we manage to access as gladys (sudo dpkg -l !/bin/sh).</span><br>

![Image](images/output-6_4.png)

<span class="font-0">Finally, gladys can execute the chown command without a password and as root.</span><br>
<span class="font-0">What can we do with chown as admin? Grant permissions to own file such as /etc/passwd.</span><br>

![Image](images/output-7_1.png)

<span class="font-0">Let's take a look at the passwd file to find out the UID and GID of the root user, which are both 0. We'll create a new user with these data. Since nano isn't recognized, we'll simply use echo to append to the end of the file.</span><br>

![Image](images/output-7_2.png)

<span class="font-0">Now we can check that the user was successfully added. Time to access as this user with the password.</span><br>
<span class="font-1">Having both root and gari the same UID (User Identifier) and GID (Group Identifier) essentially grants complete control over the system. They can perform any action, modify any file, and access any data, regardless of ownership permissions.</span><br>

![Image](images/output-7_3.png)

