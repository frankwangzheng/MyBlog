---
layout: post
title: Clean Python Setup in Ubuntu LTS
subtitle: A Simple Guide
tags: [Library]
bigimg: /img/path.jpg
---
It is a simple guide to setup a clean and maintainable python environment in Ubuntu 16.04.

### Check Default Python
Ubuntu 16.04 provides system-default Python2.7.12 and Python3.5.2, which should suffice. To install python3.6 though, use the [deadsnakes ppa](https://launchpad.net/~fkrull/+archive/ubuntu/deadsnakes): 

```shell
sudo add-apt-repository ppa:fkrull/deadsnakes
sudo apt-get update
sudo apt-get install python3.6
```

To check the location and version of the system Python, these commands are useful:

```shell
# For Python2.7
command -v python2
python2 --version

# For Python3.5
command -v python3
python3 --version

ls -l $(command -v python)
```

Note that "python" may sym-link to "python2" or "python3". To re-link, use `ln -sf` command.

### Install pip

Although Ubuntu 16.04 provides popular libraries like "python-numpy" and "python-pip" as system packages, it is not advisable to install them through `apt-get` directly. Instead, we should setup python virtual environment at this point to install other Python libraries and leave the global environment uncontaminated.

To do that, first install pip following the [official installation guide](https://pip.pypa.io/en/latest/installing/):

```shell
wget https://bootstrap.pypa.io/get-pip.py
#For Python2.7
sudo python2 get-pip.py
#For Python3.5
sudo python3 get-pip.py
```

To check the location and version of pip, these commands are useful

```shell
#For Python2.7
command -v pip2
pip2 --version

#For Python3.5
command -v pip3
pip3 --version

ls -l $(command -v pip)
```

Note that "pip" may be sym-linked to "pip2" or "pip3". To re-link, use `ln -sf` command.

### Install virtualenv and virtualenvwrapper

We can use pip to install virtualenv and virtualenvwrapper easily, do:

```shell
sudo pip install virtualenv virtualenvwrapper
```

Then we need to update "~/.bashrc" file by adding the following lines:

```shell
# virtualenv and virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

Then do `source ~/.bashrc` to reload the changes.

To make a virtual environment, do:

```shell
#For Python2.7
mkvirtualenv myenv -p python2
#For Python3.5
mkvirtualenv myenv -p python3
```

Then a folder called "myenv" will be initialized in "$WORKON_HOME".

To delete the virtual environment, do:

```shell
rmvirtualenv myenv
```

To duplicate an virtual environment, it is better to create a new virtualenv and use pip to re-install all the packages in the old environment:

```shell
#in old environment
pip freeze > requirements.txt 
#for new environment
pip install -r requirements.txt 
```

To start working on "myenv":

```shell
workon myenv
```

To quit "myenv":

```shell
deactivate
```

### Virtual environment setup with other libraries

#### Caffe

To use Python with Caffe for machine learning, PYTHONPATH needs to be set. To change the PYTHONPATH used in a virtualenv, add the following lines to your virtualenv's "myenv/bin/activate" file:

```shell
export OLD_PYTHONPATH="$PYTHONPATH"
#assuming your caffe folder is at $HOME
export PYTHONPATH="$HOME/caffe/python" 
```

To have it restored to its original value on deactivate, add the following line to the "myenv/bin/postdeactivate" script:

```shell
export PYTHONPATH="$OLD_PYTHONPATH"
```

#### OpenCV

To use Python with OpenCV for image processing, first sym-link the OpenCV python library, which usually locates in "/usr/local/lib/pythonX.X/site-packages", into the virtual environment:

```shell
#For python2.7
cd ~/.virtualenvs/myenv/lib/python2.7/site-packages/
ln -s /usr/local/lib/python2.7/site-packages/cv2.so cv2.so
#For Python3.5
cd ~/.virtualenvs/myenv/lib/python3.5/site-packages/
ln -s /usr/local/lib/python3.5/site-packages/cv2.cpython-35m-x86_64-linux-gnu.so cv2.so
```

### Reference 
* [Ubuntu 16.04: How to install OpenCV](http://www.pyimagesearch.com/2016/10/24/ubuntu-16-04-how-to-install-opencv/)
* [Properly Installing Python](http://docs.python-guide.org/en/latest/starting/installation/)