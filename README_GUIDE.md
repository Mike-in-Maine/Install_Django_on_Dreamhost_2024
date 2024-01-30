# Installing a Custom Version of Python 3

### Special thanx to Sats (not sure if that the real name) from his channel SATSifaction https://www.youtube.com/@SATSifaction great guides there including this guide which comes pretty close to what we do here. https://www.youtube.com/watch?v=hYMTvK5MpQI&t=1158s
## Setup Dreamhost part.
Why is this so complicated? Because Dreamhost is not designed to host Python or Java apps. It is a php-friendly server to host word press websites for beginner programmers and weekend-warriors. Thats it. But with the right tweaking, you can host anything on it. Their documentation is scarce, incomplete and sometimes completely misleading (on purpose???). Good luck contacting their customer support. They are nice, but they will not help. It is way too technical.
So why should I bother with Dreamhost? Because Dreamhost is the ceapest shared server you can find in the world. You can literally host 100 websites for $12 a month.
Ready? Let's get on with it!

### STEP1: Create a website on Dreamhost and an SSH user. (you can transfer a domain to it later)

Got to Website/Manage Websites/.

Add a new website button on the top right side.

Add the domain registered with dreamhost or anywhere else it without www (we will change the DNS servers late). We will use johnnybeer.com for demonstration purposes. It might take a while.

Select Shared Server.

Go to Website/SFTP Users & Files/.

Create a New User (we will use johnny for demonstration purposes). Select a name (johnny), server (shared), password you can remember (99beers99 for demonstration purposes), turn on SSH and leave it on bin/bash.

Go back to: Website/Manage Websites/ and select your website and click on Manage

Go to Website/Manage your site/Login Info

To change the user associated with this website (doesn't have to have a domain attached to it yet), activate Secure SSH access. This might take a while. Once over, you will have a new button available: Switch Username/Switch User. Select (johnny on xyzserver-abc) user from the drop down menu. Save Changes. This might take a while.

SSH user and website are now addedd to Dreamhost and you can log in via SSH.

### STEP2: Log into the server via SSH. 
What is the IP address of the website? you can find it by going to : Website/Manage Websites/Website/Manage your site/Manage. An FTP-like window will open with folders. The URL contains an IP address. Thats the one. Copy it and paste it into any SSH app like Putty or Bitwise SSH Client.

The SSH Requires a user and password which are the ones you created:

Server: IP you found on previous step (202.202.202.202 for demonstration purposes)
* User: johnny
* Pass: 99beer99

You are in. If it gives you any problems its because on Bitwise you actually have to select from the menu below Username that this connection will require a password otherwise it will not connect at all.

### STEP3: 
you should see something like this:
[server-name]$

Type: <pre lang="no-highlight"><code>```hostname```</code></pre>
Result: [server-name]$ (no you cannot change it it is your shared server).

Type: hostname -I
Result: it will give you all the IP addresses for all the shared websites you host on this server. If you look closely there is your ip too 202.202.202.202 along with its IPv6 version.

Type: ls
Result: Maildir johnnybeer.com logs

Type: cd johnnybeer.com
Type: virtualenv venv -p python3

This should create a virtual environment in your base directory

Type: ls
Result: favicon.gif  favicon.ico  venv

The virtual environment has been installed. Lets activate it:

Type: source venv/bin/activate
Result: (venv) [server-name]$

This activates the virtual environment and now we can start doing stuff in python

Lets install djsngo.

Type: pip install django

Lets install gunicorn

Type: pip install gunicorn

Lets create out django project. We will call it johnny
You should be in the same directory where venv is.

Type: python3 venv/bin/django-admin startproject johnny

Type: ls
favicon.gif  favicon.ico  johnny  venv

As you can see we have a new folder with the project we just created called johnny. Cool.

Type: cd johnny
Type: ls
Result: johnny  manage.py
You are inside the project folder where manage.py is. This is where the sqlite will go.

Type: cd johnny
You should be in this directory: __init__.py  asgi.py  settings.py  urls.py  wsgi.py

Lets change our settings.py file.

Type: nano settings.py
Move with arrows only since your mouse wont work. Carefull: any mistyped character in this file will break things.
Change the following:

Change these lines:
DEBUG = False (capital F)
ALLOWED_HOSTS ['202.202.202.202', 'www.johnnybeer.com']

Add this line:
STATIC_ROOT = '/home/johnny/johnnybeer.com/public/static/'

Save the settings.py document by pressing CTRL+x then press Y and ENTER and it will be saved.

Lets create a public folder that will contain all our static files like pictures...

Go to root directory by typing cd

you are here: favicon.gif  favicon.ico  johnny  venv

Type: mkdir public
Type: ls
Result: favicon.gif  favicon.ico  johnny  public venv

Type: mkdir public/static
This will create a static folder inside the public folder. Dreamhost needs the public folder.

NOw make sure you are in this directory: johnny  manage.py

Type: python3 manage.py collectstatic

You should see something like: 125 static files copied to '/home/johnny/johnnybeer.com/public/static'.
We hav now copied the necessary files to the static frolder inside public folder.





## Overview

Python 3 is available on Shared, VPS, and Dedicated Servers. However, if you wish to use a specific version, you can install it locally under your Shell user by following the steps outlined in this article.

## Installing Python 3
The instructions below install version 3.10.1. Make sure to change this to your chosen version in the commands below.

### Choose the version you wish to install from python.org.
Right-click on the link titled Gzipped source tarball of the version you wish to install. From the popout menu choose Copy Link Address.
Log into your server via SSH, and then run the following commands one at a time:
cd ~
mkdir tmp
cd tmp
wget https://www.python.org/ftp/python/3.10.1/Python-3.10.1.tgz
tar zxvf Python-3.10.1.tgz 
cd Python-3.10.1 
./configure --prefix=$HOME/opt/python-3.10.1
make
make install
These commands install your local version of python to /home/username/opt/python-3.10.1.
Navigate back to your user's home directory:
cd ~
View the creating and editing a file via SSH article for instructions on how to edit your existing .bash_profile. To use the new version of Python over the system default, enter the following line to your .bash_profile:
export PATH=$HOME/opt/python-3.10.1/bin:$PATH
Save and close the file, and then return to your shell. Run the following command to update this file:
. ~/.bash_profile
Check which version of Python you're now using by entering the following command:
which python3
/home/username/opt/python-3.10.1/bin/python3
You can also check the version:

python3 --version
Python 3.10.1
If there is no response then the newly downloaded copy is not being used. Most often this is due to the .bash_profile not being updated correctly. Try logging out and back in again. If necessary, repeat the steps above.

## Install Django using virtualenv
### Overview
Using virtualenv to install Django is recommended on DreamHost Shared and VPS plans since your user doesn't have access to install into shared directories. 
When you use virtualenv, you create an isolated environment with its own installation directories which your user has full permissions to. This allows you to install a custom version of Python and its different packages which is not connected to the global installation on the server. This also solves the issue with permissions when installing software.

### Create and activate your virtual environment using a custom Python version (like in our case).
Link to the original article: https://help.dreamhost.com/hc/en-us/articles/115000695551-Installing-and-using-Python-s-virtualenv-using-Python-3

Be aware that you may need to reinstall Python following a server operating system upgrade. In other words your custom Python will only stay on until Dreamhost doeas a server upgrade (like they often do), and after that you will need to re-install your custom python again because the server will revert back to the default python. I'm not sure why they encourage to install custom python at all...

When working with virtual environments in Python, it's common to use a custom version of Python rather than the server's version. To create a new virtual environment using your custom installed version of Python, follow these steps:

The following steps use Python version 3.10.1. Make sure to use the version you installed.

Make a note of the full file path to the custom version of Python you just installed. If you've followed the instructions in the installation article, the full path is:
which python3
/home/username/opt/python-3.10.1/bin/python
Navigate to your site's directory, where you'll create the new virtual environment:
cd ~/example.com
Update your .bash_profile
. ~/.bash_profile
Create the virtual environment while you specify the version of Python you wish to use. The following command creates a virtualenv named 'venv' and uses the -p flag to specify the full path to the Python3 version you just installed:
You can name the virtualenv anything you like.

virtualenv -p /home/username/opt/python-3.10.1/bin/python3 venv

Running virtualenv with interpreter /home/username/opt/python-3.10.1/bin/python3
Using base prefix '/home/username/opt/python-3.10.1'
New python executable in /home/username/example.com/env/bin/python3
Also creating executable in /home/username/example.com/env/bin/python
Installing setuptools, pip, wheel...done.
This command creates a local copy of your environment specific to this website. While working on this website, you should activate the local environment in order to make sure you're working with the right versions of your tools and packages.
You may see the following error when installing.

setuptools pip failed with error code 1` error
If so, run the following:

pip3 install --upgrade setuptools
Try again and you should be able to install without an error.

To activate the new virtual environment, run the following:
source venv/bin/activate
The name of the current virtual environment appears to the left of the prompt. For example:
To verify the correct Python version, run the following:
python -V
Python 3.10.1
Any package that you install using pip is now placed in the virtual environments project folder, isolated from the global Python installation.
