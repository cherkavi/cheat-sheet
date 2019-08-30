# [python3 readiness](http://py3readiness.org/)

## proxy 
```
import os

proxy = 'http://<user>:<pass>@<proxy>:<port>'
os.environ['http_proxy'] = proxy 
os.environ['HTTP_PROXY'] = proxy
os.environ['https_proxy'] = proxy
os.environ['HTTPS_PROXY'] = proxy
```

## package manager easy_install
### using it from cmd
```
%PYTHON%/Scripts/easy_install.exe <package name>
```
### using easy_install from script
```
from setuptools.command import easy_install
# install package
easy_install.main( ["termcolor"] )
# update package
easy_install.main( ["-U","termcolor"] )
```

## package manager pip
### find package by name
```
pip search {search key}
```

### install pip
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```
issue:
```
ImportError: cannot import name 'sysconfig'
```
solutionpip install
```
sudo apt install python3-distutils
```

### upgrade pip
```
pip3 install --upgrade pip
pip install -U pip
```
debian
```
apt-get install -y --no-install-recommends python3-pip
```

### pip install with proxy
```
pip install --proxy=http://proxy.muc:8080
```

### pip install with specific proxy
```
pip install --index-url http://cc-artifactory.mynetwork.net my_own_package
```
### install package into home of current user ( do not use for virtual environment )
```
pip install --user .
```

### list of packages
```
pip list
```
### path to external artifacts, external index, pip configuration
```bash
$ ​cat /etc/pip.conf 
```
```properties
[global]
index-url = https://cc-artifactory.myserver.net/artifactory/api/pypi/adp-pypi-virtual/simple
```

### install certain version of artifact
```
pip install tornado==2.1.1
```

### create virtual environment, dedicated env
```
pip install virtualenv
```
```
python3 -m venv virtual_env
source virtual_env/bin/activate
# deactivate
pip install wheel 
python3 
```

### install artifact from git
```
pip install git+https://github.com/django-extensions/django-extensions
pip install git+https://github.com/django-extensions/django-extensions.git
pip install -e git+https://github.com/django-extensions/django-extensions.git#egg=django-extensions
pip install https://github.com/django/django/archive/stable/1.7.x.zip
pip install git+ssh://git@github.com/myuser/foo.git@my_version
```

### install package to specific folder
```
pip install --target=/home/user/my/python/packages package_name
export PYTHONPATH=$PYTHONPATH:"/home/user/my/python/packages"
```
### load package from specific folder inline
```
import sys
sys.path.append("/path/to/your/my_extra_component")
import extra_component
```

### update package
```
pip install tornado --update
```

### remove package, uninstall package
```
pip uninstall {package name}
```

### list of all installed libraries, installed modules
```
pip freeze
```
```
import sys
sys.modules
```

### list of all folders with source code, installed packages
```
import sys
print(sys.prefix)
```
for easy_install, pip
```
import site
print(site.getsitepackages())
```

### using pip from interpreter ( install wheels package)
```
import pip
pip.__version__
pip.main("install", "wheels")
```
fix for version 9 and 10
```
error message: AttributeError: 'module' object has no attribute 'main'
```
solution:
```
try:
    from pip import main as pipmain
except:
    from pip._internal import main as pipmain
pipmain(["install", "wheels"]);
```

### import package by string name
```
target = __import__("data-migration")
```

### import package in protected block
```
try:
    import json
except ImportError:
    import simplejson as json
```

### path to python, obtain python interpreter path
```
import sys
sys.executable
```
also check 
```
sys.modules
```

### execute command inline, base64 example
```
python -c "import base64;print(base64.encodestring('hello'.encode()));"
```

### execute string as commands
```
a=10
eval(" print(a)")
```

### find vs index ( string find, string index )
```
find - return -1 if not found
index - throw exception if not found
```

### list of frameworks
```
awesome-python
```

### user libraries can be placed into the folder
```
python -m site --user-site
```

### help
```
help contextlib
pydoc contextlib
```

### assign
```
b = "some value" if True else "another value"
```

### need to use
```
enumerate, zip
```

### special methods
```
__call__  <class instance>()
```

### build rpm: python setup.py bdist_rpm
```
	rpm -ba --define _topdir ...
	-ba: unknown option
	error: command 'rpm' failed with exit status 1
```

```
need to execute: yum install rpm-build
```

### build package usind pants with special folder
```
# BUILD PEX
PANTS_PATH="../../../.."
PANTS_OUTPUT=$CURRENT_PATH/dist
$PANTS_PATH/pants --pants-distdir=$PANTS_OUTPUT binary src/path/to/package:package-name

```

### (debug with cli)[https://docs.python.org/3/library/pdb.html]
```python
import pdb
# ...
pdb.set_trace()
```
execute in debug
```sh
python3 -m pdb myscript.py
```
commands
```
until 99999
next
return
```

## install packages
### selenium, virtualdisplay
```
sudo pip install selenium
sudo pip install xvfbwrapper
sudo pip install pyvirtualdisplay
sudo apt-get install xvfb
wget https://github.com/mozilla/geckodriver/releases/download/v0.21.0/geckodriver-v0.21.0-linux64.tar.gz
chmod +x geckodriver
sudo cp geckodriver /usr/local/bin/
```

### [virtual environment automation, tox tool](https://tox.readthedocs.io/en/latest/)

## IDEA
### standard modules not found 
```
Have you set up a python interpreter facet?
Open Project Structure CTRL+ALT+SHIFT+S

Project settings -> Facets -> expand Python click on child -> Python Interpreter

Then:
Project settings -> Modules -> Expand module -> Python -> Dependencies -> select Python module SDK
```
