VirtualEnv and Ipython Notebook
===============================

How to set up an virtual environment  on a host, working together with IPython Notebook

##Virtual Environment

Using a virtual environment can be very interesting in several situations:

* You have different applications that require different versions of the same library
* You want a stable, immutable environment for an application that surely is working
* You need to install packages without the need to ask for permissions (on a shared host)


*VirtualEnv page:*
https://virtualenv.pypa.io/en/latest/index.html


*Downloads:*
https://pypi.python.org/pypi/virtualenv


##Download and activation

First access the host using a *ssh* conection:

<pre>
ssh username@host.foo
</pre>

Create a folder to keep all your virtual enviroments:

<pre>
mkdir myfolder
cd myfolder
</pre>

Install the current virtualenv version (10/12/2014):

<pre>
curl -O https://pypi.python.org/packages/source/v/virtualenv/virtualenv-1.11.6.tar.gz
tar xvfz virtualenv-1.11.6.tar.gz
cd virtualenv-1.11.6
python virtualenv.py ~username/myfolder/Env01
</pre>

This should create a new virtual enviroment at ''username/myfolder/Env01''

Activate the virtual environment:

<pre>
cd ~username/myfolder/Env01/bin
bash
source activate
</pre>

After activation your console should look like this:

**(Env01)username@host:~/myfolder/Env01/bin$**

This means we are currently using the virtual environment "Env01"

##IPython

Now we're able to install IPython (together with other essential libraries) on our virtual enviroment:

<pre>
pip install numpy scipy tornado pyzmq pandas ipython pygments matplotlib jinja2
</pre>

The download and installation should take a few minutes.

After the installation is complete, we can start to configure the Notebook server.

##Notebook

First of all, we need to create a hashed password to use with the notebook. Open IPython and follow:

<pre>
from IPython.lib import passwd
passwd()
</pre>

You should get a hashed version of your password. For ''test01'' it is as follows:

<pre>
'sha1:a148e2c2438c:64446b70d64d12ba09fcf5ad75ac985062cbe0d5'
</pre>


Next, we need to create an IPython configuration file, and use vim to edit. 

<pre>
ipython profile create server01
</pre>

A profile named ''server01'' was created.

Then use **vim** to edit the config file

* *To start an insertion on vim press "i"*
* *To save and quit vim first press "escape" then type ":wq"*
* *To quit without saving press "escape" and then "q!"*

<pre>
vim ~/.ipython/profile_server01/ipython_notebook_config.py
</pre>

Within the vim editor, set the file like:

<pre>
c = get_config()

c.IPKernelApp.pylab = 'inline'
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.password = u'sha1:yourhashedpassword'
c.NotebookApp.port = 9876

c.NotebookApp.base_project_url = '/notebookname/'
c.NotebookApp.base_kernel_url = '/notebookname/'
c.NotebookApp.webapp_settings = {'static_url_prefix':'/notebookname/static/'}
</pre>

Use the hashed password created before and a random port number. Also use choose a name for your access on "notebookname"



You now should be able to start the Ipython notebook:

<pre>
ipython notebook --profile=server01
</pre>

Then open your browser and access:

<pre>
http://bioclusts01.bioclust.biologie.ens.fr:9876/notebookname/
</pre>


Create a new notebook and test the common libraries and inline plotting with the following code:

<pre>
import numpy as np
import matplotlib.pyplot as plt


N = 50
x = np.random.rand(N)
y = np.random.rand(N)
colors = np.random.rand(N)
area = np.pi * (15 * np.random.rand(N))**2 # 0 to 15 point radiuses

plt.scatter(x, y, s=area, c=colors, alpha=0.5)
plt.show()
</pre>
