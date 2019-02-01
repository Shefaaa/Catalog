
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

<h3>Install and configure Apache and pthon mod_wsgi</h3>
<ol>
<li>Run <code>$ sudo apt-get install apache2</code> to install apache2</li>
<li>Go to <a href="http://13.232.218.51" rel="nofollow">http://13.232.218.51/</a>, if Apache is working correctly, a <strong>Apache2 Ubuntu Default Page</strong> will show up</li>
<li>Run <code>$ sudo apt-get install libapache2-mod-wsgi python-dev</code> To Install the mod_wsgi</li>
<li>Run <code>$ sudo a2enmod wsgi</code> To Enable mod_wsgi</li>
<li>Run <code>$ sudo service apache2 restart</code> To Restart Apache</li>
<li>Run <code>$ python</code> To Check if Python is installed</li>  
</ol>

<h3>Install PostgreSQL</h3>
<ol>
<li>Run <code>$ sudo apt-get install postgresql</code></li>
<li>Make sure PostgreSQL does not allow remote connections</li>
<li>Open file: <code>$ sudo nano /etc/postgresql/9.5/main/pg_hba.conf</code></li>
<li>Check to make sure it looks like this:
<pre><code># Database administrative login by Unix domain socket
local   all             postgres                                peer

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
</code></pre>
</li>
</ol>

<h3>Create new PostgreSQL user called catalog</h3>
<ol>
<li>Run <code>$ sudo su - postgres</code> To Switch to PostgreSQL defualt user postgres</li>
<li>Run <code>$ psql</code> To Connect to PostgreSQL</li>
<li>Create user <strong>catalog</strong> with LOGIN role: <code># CREATE ROLE catalog WITH PASSWORD 'catalogdb';</code></li>
<li>Allow user to create database tables: <code># ALTER USER catalog CREATEDB;</code></li>
<li>Create database: <code># CREATE DATABASE catalog WITH OWNER catalog;</code></li>
<li>Connect to database <strong>catalog</strong>: <code># \c catalog</code></li>
<li>Revoke all the rights: <code># REVOKE ALL ON SCHEMA public FROM public;</code></li>
<li>Grant access to <strong>catalog</strong>: <code># GRANT ALL ON SCHEMA public TO catalog;</code></li>
<li>Exit psql: <code>\q</code>
10.Exit user <strong>postgres</strong>: <code>exit</code></li>
</ol>

<h3>Install git</h3>
<p>Run <code>$ sudo apt-get install git</code> To install git</p>

<h3>Clone and setup the Item Catalog Project</h3>
<ol>
<li>Create dictionary: <code>$ mkdir /var/www/catalog</code></li>
<li>CD to this directory: <code>$ cd /var/www/catalog</code></li>
<li>Change the ownership: <code>$ sudo chown -R grader:grader catalog/</code></li>  
<li>Clone the catalog app: <code>$ sudo git clone Repository-URL catalog</code></li> 
</ol>
<p><strong>create .wsg file </strong> <code>$ sudo nano catalog.wsgi</code></p>
<pre lang="$"><code> 
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog")
from catalog import app as application
application.secret_key = 'super_secret_key'
</code>
</pre>

<p><strong>create the virtual environment </strong></p>
<pre lang="$"><code> 
$ sudo pip install virtualenv
$ sudo vitrualenv venv
$ source venv/bin/activate
$ sudo chmod -R 777 venv
</code>
</pre>

<p><strong>Install the required package for the Flask </strong></p>
<pre lang="$"><code> 
$ sudo apt-get install python-pip
$ sudo pip install flask
$ sudo pip install httplib2 oauth2client sqlalchemy psycopg2
$ sudo pip install requests
$ sudo pip install --upgrade oauth2client
$ sudo apt-get install libpq-dev
$ sudo pip install sqlalchemy_utils
</code>
</pre>

<p>Deactivate the virtual environment by the command <strong>deactivate</strong></p>

<p>change to the <strong>/var/www/catalog/catalog</strong> directory</p>
<p>Rename the project.py to __init__.py <code>mv project.py __init__.py</code></p>
<p>update the path of the client_secrets.json file <strong>/var/www/catalog/catalog/client_secrets.json</strong></p>
<p>change <strong>app.run(host='0.0.0.0', ort=8000, debug=True)</strong> to <strong>app.run()</strong></p>
<p>Run database and seeds files <strong>python database_setup.py  python seeds.py</strong></p>

<h3>Setup and enble a virtual host</h3>
<ol>
<li>Create file: <code>$ sudo touch /etc/apache2/sites-available/catalog.conf</code></li>
<li>Add the following to the file:</li>
</ol>
