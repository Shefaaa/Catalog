
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


 
