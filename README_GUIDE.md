# Installing a Custom Version of Python 3

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

### Create and activate your virtual environment.
Link to the original article: https://help.dreamhost.com/hc/en-us/articles/115000695551-Installing-and-using-Python-s-virtualenv-using-Python-3
