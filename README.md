
# Linux-Server-Configuration Project
In this project, the Catalog Item Web Application Project will be hosted by a Ubuntu Linux server on an Amazon Lightsail instance. A series of instructions will be presented below. You can visit <a href="http://ec2-13-232-218-51.ap-south-1.compute.amazonaws.com">http://ec2-13-232-218-51.ap-south-1.compute.amazonaws.com/</a> for the deployed website.
<ul>
  <li>Public IP address: 13.232.218.51</li>
  <li>SSH port: 2200</li>
</ul>
<h3>Create instance on Amazon Lightsail</h3>
<ol>
<li>Create an AWS account on <a href="https://lightsail.aws.amazon.com">Lightsail</a></li>
<li>Click <strong>Create instance</strong> button on the home page</li>
<li>Select <strong>Linux/Unix</strong> platform</li>
<li>Select <strong>OS Only</strong> and <strong>Ubuntu</strong>Platform</li>
<li>Select an instance plan</li>
<li>Name your instance</li>
<li>Click <strong>Create</strong> button and wait for the instance to run</li>
</ol>


<h3>System setup and how to view this project</h3>
<ul>
  <li>Download and install Vagrant</li>
  <li>Download and install Virtual Box</li>
  <li>Clone this repository</li>
  <li>Run <code>vagrant up</code> command to start up the VM</li>
  <li>Run <code>vagrant ssh</code> command to log into the VM.</li>
  <li><code>cd /vagrant </code>to change to your vagrant directory</li>
  <li>Move inside the catalog folder <code>cd /vagrant/catalog</code></li>
  <li>Initialize the database <code>$ Python database_setup.py</code></li>
  <li>Populate the database with some initial data <code>$ Python seeds.py</code></li>
  <li>Run <code>python project.py</code></li>
  <li>Browse the App at this URL <code>http://localhost:8000</code></li>
</ul>

<h3>JSON endpoints:</h3>
<strong>Returns JSON of all categories</strong>
<pre><code>/catalog/categories/json</code></pre>

<strong>Returns JSON of all the items in a specific category</strong>
<pre><code>/catalog/&lt;int:category_id&gt;/items/json</code></pre>

<strong>Returns JSON of item setails in a specific category</strong>
<pre><code>/catalog/&lt;int:item_id&gt;/itemdetails/json</code></pre>


<h3>Helpful Resources</h3>
<ul>
  <li><a href="https://www.python.org/dev/peps/pep-0008/">PEP8 style guide</a></li>
</ul>  
