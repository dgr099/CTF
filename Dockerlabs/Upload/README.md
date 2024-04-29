
## <span class="font-0">Upload</span><br>

![Image](images/output-1_1.png)

<span class="font-1">As always we start with the deployment of the machine and checking that we have connectivity to it with a simple ping.</span><br>


![Image](images/output-1_2.png)


<span class="font-1">Next step will be the enumeration of services and ports, for this we will use nmap.</span><br>

![Image](images/output-1_3.png)

<span class="font-1">The only port we see open is port 80 which is running an Apache http server.</span><br>
<span class="font-1">We are going to apply fuzzing on this one using gobuster.</span><br>
![Image](images/output-2_1.png)

<span class="font-1">From the results, the two marked directories /uploads and /upload.php stand out, from the names we can already intuit what we can find./span><br>
![Image](images/output-2_2.png)

<span class="font-1">Let's try uploading a file and then accessing it. If files uploaded to the server are not properly validated, they could include malicious code or scripts that could be used to compromise the security of the server or other users accessing those files. By uploading executable files or scripts to the server, an attempt could be made to exploit remote code execution. Let's check if we can actually see what we upload and check if the characters are cleaned up in case an XSS is possible.</span><br>
![Image](images/output-3_1.png)

![Image](images/output-3_2.png)

<span class="font-1">It seems to be cleaning up the characters so an xss doesn't seem like an option. Let's then go with the first option when we are allowed to upload files which is to try to launch malicious php code.</span><br>
<span class="font-1">PHP (Hypertext Preprocessor) is a server-side scripting language widely used for web development. It integrates with HTML to generate dynamic content and interactive web pages..</span><br>
<span class="font-1">Let's check if we are indeed able to execute php code.</span><br>
![Image](images/output-4_1.png)

<span class="font-1">Once checked we are really close to overcoming the machine.</span><br>
<span class="font-1">let's create a php payload that allows us to set up a reverse shell.</span><br>

![Image](images/output-4_2.png)

![Image](images/output-4_3.png)

<span class="font-1">We can see how we are under the session of the user www-data. In this case,</span><br>
<span class="font-3">www-data</span>
<span class="font-1">can execute the command</span>
<span class="font-3">/usr/bin/env</span>
<span class="font-1">with root privileges</span>
<span class="font-1">without the need to enter a password.</span><br>
<span class="font-1">in other words, we can run</span>
<span class="font-3">/bin/sh</span>
<span class="font-1">with the root privileges inherited from sudo.</span><br>
<span class="font-1">And with this we have already solved the machinw.</span><br>

![Image](images/output-5_1.png)

