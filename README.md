
# Linux-Server-Configuration Project
In this project, the Catalog Item Web Application Project will be hosted by a Ubuntu Linux server on an Amazon Lightsail instance. A series of instructions will be presented below. You can visit <a href="http://ec2-13-232-218-51.ap-south-1.compute.amazonaws.com">http://ec2-13-232-218-51.ap-south-1.compute.amazonaws.com/</a> for the deployed website.
<ul>
  <li>Public IP address: 13.232.218.51</li>
  <li>SSH port: 2200</li>
</ul>
<h3>Create instance on Amazon Lightsail</h3>
<ol>
<li>Create an AWS account on <a href="https://lightsail.aws.amazon.com">Amazon Lightsail</a></li>
<li>Click <strong>Create instance</strong> button on the home page</li>
<li>Select <strong>Linux/Unix</strong> platform</li>
<li>Select <strong>OS Only</strong> and <strong>Ubuntu</strong>Platform</li>
<li>Select an instance plan</li>
<li>Name your instance</li>
<li>Click <strong>Create</strong> button and wait for the instance to run</li>
</ol>


<h3>SSH into the Server</h3>
<ol>
<li>Download private key from the <strong>SSH keys</strong> section in the <strong>Account</strong> section on Amazon Lightsail.</li>
<li>Move the downloaded private key file into the local folder ~/.ssh and rename it as <strong>lightsail_key.pem</strong> </li>
<li>Set file permission as owner only : <code>$ chmod 600 ~/.ssh/lightsail_key.pem</code></li>
<li>Connect to the instance via SSH: <code>$ ssh -i ~/.ssh/lightsail_key.rsa ubuntu@13.232.218.51</code></li>
</ol>

<h3>Update all installed packages on the Server</h3>
<ol>
<li>Run <code>sudo apt-get update</code> to update the packages</li>
<li>Run <code>sudo apt-get upgrade</code> to install the newest versions of packages</li>
<li>Run <code>sudo apt-get dist-upgrade</code> for future updates</li>
</ol>

<h3>Change the SSH port from 22 to 2200</h3>
<ol>
<li>Run <code>$ sudo nano /etc/ssh/sshd_config</code> to edit the configuration file</li>
<li>Change the port from <strong>22</strong> to <strong>2200</strong></li>
<li>Save and exit the file</li>
<li>Run <code>$ sudo service ssh restart</code> to Restart SSH</li>
</ol>

<h3>Allow Ports 80(TCP), 123(UDP), 2200(TCP) and deny the default port 22</h3>
<ol>
  <li>Update the firewall configuration on Amazon Lightsail website under <strong>Networking</strong>. Delete default SSH port 22 and add port 80, 123, 2200</li>
  <li>You can now ssh in via the new port 2200: <code>$ ssh -i ~/.ssh/lightsail_key.pem ubuntu@13.232.218.51 -p 2200</code></li>
</ol>  

<h3>Configure the firewall</h3>
<ol>
<li>Run <code>$ sudo ufw status</code> To Check firewall status</li>
<li>Run <code>$ sudo ufw default deny incoming</code> To deny all incomings</li>
<li>Run <code>$ sudo ufw default allow outgoing</code> To allow all outgoings</li>
<li>Run <code>$ sudo ufw allow 2200/tcp</code> To Allow incoming TCP packets on port 2200 to allow SSh</li>
<li>Run <code>$ sudo ufw allow www</code> To Allow incoming TCP packets on port 80 to allow www</li>
<li>Run <code>$ sudo ufw allow 123/udp</code> To Allow incoming UDP packets on port 123 to allow NTP </li>
<li>Run <code>$ sudo ufw deny 22</code> To Close port 22</li>
<li>Run <code>$ sudo ufw enable</code> To Enable firewall</li>
<li>Run <code>$ sudo ufw status</code> To Check out current firewall status</li>
</ol>

<h3>Create a new user named grader and give it sudo access</h3>
<ol>
<li>Run <code>$ sudo adduser grader</code> To Create a new user account </li>
<li>Enter the password and filled the user information</li>  
<li>Run <code>$ sudo visudo</code> To edit the sudoers</li>
<li>Add code <code>grader ALL=(ALL:ALL) ALL</code>. under this line <code>root ALL=(ALL:ALL) ALL</code></li>
<li>Save and exit</li>
<li>Run <code>su - grader</code> enter the password then <code>sudo -l</code> and enter the password again To verify that grader has sudo permission </li>  
<li>run <code>sudo apt-get install finger</code> To install finger to see the users on this server</li>  
</ol>

<h3>Set SSH login using keys</h3>
<ol>
<li>Create an SSH key pair for <strong>grader</strong> using the <code>ssh-keygen</code> Save it in <code>~/.ssh/grader</code> path</li>
<li>Deploy public key on development environment
<ul>
<li>On your local machine, read the generated public key
<code>cat ~/.ssh/grader.pub</code></li>
<li>On your virtual machine</li>
</ul>
<pre lang="$"><code> 
   $ cd /home/grader
   $ mkdir .ssh
   $ touch .ssh/authorized_keys
   $ nano .ssh/authorized_keys
</code></pre>
<ul>
<li>Copy the public key to this <em>authorized_keys</em> file on the virtual machine and save</li>
</ul>
</li>
<li>Run <code>chmod 700 .ssh</code> and <code>chmod 644 .ssh/authorized_keys</code> on your virtual machine to change file permission</li>
<li>Restart SSH: <code>$ sudo service ssh restart</code></li>
<li>Now you are able to login in as grader: <code>$ ssh -i ~/.ssh/grader -p 2200 grader@13.232.218.51</code></li>
<li>You will be asked for grader's password. To dusable it, open configuration file: <code>$ sudo nano /etc/ssh/sshd_config</code></li>
<li>Change <code>PasswordAuthentication yes</code> to <strong>no</strong></li>
  <li>Disable SSH login to the root change <strong>PermitRootLogin</strong> to <strong>no</strong></li>  
<li>Restart SSH: <code>$ sudo service ssh restart</code></li>
</ol>

<h3>Configure the local timezone to UTC</h3>
<ol>
<li>Run <code>$ date</code> To check the timezone</li>
<li>Run <code>timedatectl set-timezone UTC</code> to set timezone to UTC</li>
</ol>


 
