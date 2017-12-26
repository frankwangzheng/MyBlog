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

Note that "python" may sym-link to "python2" or "python3". To ensure "python3" as the default "python", re-link "/usr/bin/python" using `ln -sf /usr/bin/python /usr/bin/python3` command.

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

Note that "pip" may be sym-linked to "pip2" or "pip3". To ensure "pip3" is the default "pip" , re-link "pip" using `ln -sf /usr/local/bin/pip /usr/local/bin/pip3` command.

### Install virtualenv and virtualenvwrapper

We can use pip to install virtualenv and virtualenvwrapper easily, do:

```shell
sudo pip install virtualenv virtualenvwrapper
```
Support pip is linked to "pip3"
Then we need to update "~/.bashrc" file by adding the following lines:

```shell
# virtualenv and virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

Then do `source ~/.bashrc` to reload the changes.

To make a virtual environment, do:

```shell
#For Python2.x
mkvirtualenv myenv -p python2
#For Python3.x
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

### Link external libraries with virtualenv


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
ln -s /usr/local/lib/python2.7/dist-packages/cv2.so cv2.so
#For Python3.5
cd ~/.virtualenvs/myenv/lib/python3.5/site-packages/
ln -s /usr/local/lib/python3.5/dist-packages/cv2.cpython-35m-x86_64-linux-gnu.so cv2.so
```


#### Jupyter Notebook


First install [Jupyter Notebook](http://jupyter.org/index.html) by executing `pip install jupyter`. Then add [Python 2 and Python 3 kernels](https://ipython.readthedocs.io/en/latest/install/kernel_install.html) for the execution of Jupyter Notebooks by executing:

```shell
python2 -m pip install ipykernel
python2 -m ipykernel install --user --name myenv --display-name "Python2 (myenv)"

python3 -m pip install ipykernel
python3 -m ipykernel install --user --name myenv --display-name "Python3 (myenv)"
```

Lastly run jupyter notebook by executing `jupter notebook filename`.


### Reference 
* [PyImageSearch's Installation Guide](http://www.pyimagesearch.com/2016/10/24/ubuntu-16-04-how-to-install-opencv/)
* [Properly Installing Python](http://docs.python-guide.org/en/latest/starting/installation/)