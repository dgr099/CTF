# Vacaciones

![Image](images/output-1_1.png)

<span class="font-0">The first step will be to deploy the docker container and check if we can establish</span>
<span class="font-0">connection by launching a ping to the machine.</span>

![Image](images/output-1_2.png)

<span class="font-0">Enumeration of ports and services using nmap</span><br>

![Image](images/output-1_3.png)

<span class="font-0">Let's try to access the apache server and see what we find.</span>
<span class="font-0">It seems to be empty, but inspecting the page we can see that we find a</span>
<span class="font-0">user, actually 2, Camilo and Juan.</span><br>
<span class="font-0">Let's continue trying with directory enumeration using gobuster.</span><br>

![Image](images/output-1_4.png)


![Image](images/output-2_1.png)

<span class="font-0">We found that the javascript directory may be of interest but there's really nothing</span>
<span class="font-0">that makes us think about exploiting the application from there.</span>
<span class="font-0">Let's see if we are able to enumerate SSH users.</span>


![Image](images/output-2_2.png)


![Image](images/output-3_1.png)

![Image](images/output-3_2.png)

<span class="font-0">It seems to throw false positives and we cannot trust those values.</span>
<span class="font-0">Let's try a dictionary attack for Camilo or Juan, with hydra we launch both.</span><br>
<span class="font-0">We found the password for camilo password1, now we can access via ssh.</span>
![Image](images/output-4_1.png)

<span class="font-0">Once in the ssh we notice that there is a third user named pedro and</span>
<span class="font-0">that camilo cannot execute any command as sudo.</span>
<span class="font-0">We remember that the server message said something about an important message so</span><br>


![Image](images/output-4_2.png)

<span class="font-0">let's check messages and emails for more information.</span>

![Image](images/output-5_1.png)

![Image](images/output-5_2.png)

![Image](images/output-5_3.png)

![Image](images/output-5_4.png)


<span class="font-0">Indeed, there was an important message such as the password of another user, let's</span><br>
<span class="font-0">try with Juan since he is the one who said he had an important message.</span>
<span class="font-0">From Juan we can indeed execute commands as sudo, with this we can already exploit the</span><br>
<span class="font-0">machine and escalate privileges.</span>
<span class="font-0">As a curiosity in the shadow file we see how there are more users and that pedro has</span><br>
<span class="font-0">an entry in the table with his password.</span><br>
![Image](images/output-6_1.png)

<span class="font-0">Starting with $6$ we can guess that it is sha-512, a type of hash that is very resistant</span><br>
<span class="font-0">that would not be easy to break.</span><br>
</span>
