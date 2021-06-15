# [python3 readiness](http://py3readiness.org/)
# [list of frameworks](https://awesome-python.com/)

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
### [find package in browser](https://pypi.org/)
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
solution
```
# pip install
# sudo apt install python3-distutils
sudo apt install python3-pip
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

### pip install with proxy, pip install proxy, pip proxy, pip via proxy
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

### setup.py
#### install
```sh
python setup.py install
```
#### uninstall
```
python setup.py install --record list_of_files.txt
cat list_of_files.txt | xargs sudo rm -rf
```

### list of packages
```
pip list
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
pip.main(["install", "wheels"])
#pip.main("install", "wheels")
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
# create 
#python3 -m virtualenv venv
virtualenv venv

# activate
source venv/bin/activate

# commands will be executed into virtual env
pip install wheel 
python3 

# exit from virtual environment
deactivate
```

### install list of artifacts
```sh
pip install -r requirements.txt
# sometimes after installation not working
# 'pip list' & 'pip freeze' are not consistent
cat requirements.txt | xargs -I {} ./pip3 install {}
```
where requirements.txt is:
```
-r requirements-base.txt
docker==3.7.0
enum34==1.1.6
flask-restful==0.3.7
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

### print dependency tree
```python3
pip install pipdeptree
pipdeptree
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

### create executable environment
```sh
pex --python=python3 flask requests tornado -o samplepkg.pex
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

# sql, sqlgenerator, codegenerator, sqlalchemy
```
pip3 install sqlacodegen
sqlacodegen sqlite:///db.sqlite > generated-code.txt
sqlacodegen postgresql:///some_local_db
sqlacodegen mysql+oursql://user:password@localhost/dbname

# Flask approach
pip3 install flask-sqlacodegen
flask-sqlacodegen --flask sqlite:///db.sqlite > generated-code.txt
flask-sqlacodegen --flask mysql+pymysql://admin:admin@localhost:3310/masterdb > generated-code.txt
```
# sql client, console mysql client, mysql console, db cli
[commands](https://www.mycli.net/commands)
```
apt-get install mycli
pip install -U mycli
mycli --user my_user --password my_password --host my_host.com --port 3310 --database my_database --execute 'show tables'

# for activating multiline mode 'F3'
```
## Code Style code Formatter
```sh
sudo apt-get remove black && python3 -m pip install black==20.8b1
black --line-length 100
```

## Test coverage
```sh
# cli tool
pip3 install tox 
# plugin for PyCharm: pytest-cov
```


# Alembic 
![migration schema](https://i.postimg.cc/tJSJWfFc/alembic.png)
## migration
```bash
# step #1 
alembic init datastorage_mysql

# step #2 
# check your "sqlalchemy.url" in "ini file"
# check your "target_metadata" in "env.py"
# target_metadata = Base.metadata # from database.models import Base

# step #3
# generate current difference between model and 
export PYTHONPATH=$PYTHONPATH:$(pwd)/src
alembic --config alembic-local.ini revision --autogenerate -m "baseline" 2>out.txt

alembic --config alembic-local.ini history

alembic --config alembic-local.ini upgrade head

```

## reset alembic
```sql
delete from alembic_version where 1=1;
```
```bash
rm datastorage_mysql/versions/*
```

## supplementary steps
```bash
# step set marker in DB
# create table if not exists alembic_version
alembic --config alembic-local.ini current

# upgrade to specific revision
alembic --config alembic-local.ini upgrade cfaf8359a319 --sql 
```

# GUI
## PySimpleGUI
```
sudo apt-get install python3-tk
pip3 install PySimpleGUI
```

# Web frameworks lightweight
* [bottle](http://bottlepy.org/)  
* [CherryPy](http://www.cherrypy.org/)  
* [Falcon](http://falconframework.org/)  
* [FastAPI](https://www.starlette.io/)  
* [Pyramid](https://trypyramid.com/)  
* [Sanic](https://sanic.readthedocs.io/en/latest/)  
* [Tornado](http://www.tornadoweb.org/)  
* [Streamlit](https://docs.streamlit.io/en/stable/)
* [viola](https://pypi.org/project/viola/)
