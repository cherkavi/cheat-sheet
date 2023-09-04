# Python
* [python3 readiness](http://py3readiness.org/)  
* [list of frameworks](https://awesome-python.com/)  

## proxy 
```python
import os

proxy = 'http://<user>:<pass>@<proxy>:<port>'
os.environ['http_proxy'] = proxy 
os.environ['HTTP_PROXY'] = proxy
os.environ['https_proxy'] = proxy
os.environ['HTTPS_PROXY'] = proxy
```

## [package manager poetry](https://python-poetry.org/)  
## package manager easy_install
### using it from cmd
```sh
%PYTHON%/Scripts/easy_install.exe <package name>
```
### using easy_install from script
```python
from setuptools.command import easy_install
# install package
easy_install.main( ["termcolor"] )
# update package
easy_install.main( ["-U","termcolor"] )
```

## package manager pip
### [find package in browser](https://pypi.org/)
### find package by name
```sh
pip search {search key}
```

### install pip
```sh
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```
issue:
```
ImportError: cannot import name 'sysconfig'
```
solution
```sh
# pip install
# sudo apt install python3-distutils
sudo apt install python3-pip
```

### upgrade pip
```sh
pip3 install --upgrade pip
pip install -U pip
```
debian
```sh
apt-get install -y --no-install-recommends python3-pip
```

### setup pip remote index
```sh
NEXUS_HOST=some.host.com
python3 -m pip config --user set global.index-url https://${NEXUS_USER}:${NEXUS_PASS}@${NEXUS_HOST}
python3 -m pip config --user set global.trusted-host ${NEXUS_HOST}
```

## [pip install git svn folder](https://pip.pypa.io/en/stable/cli/pip_install/)
### pip install with proxy, pip install proxy, pip proxy, pip via proxy
```sh
pip install --proxy=http://proxy.muc:8080
```

### pip install with specific proxy
```sh
pip install --index-url http://cc-artifactory.mynetwork.net my_own_package
```
### install package into home of current user ( do not use for virtual environment )
```sh
pip install --user .
```
### pip install from package install from zip
```sh
pip3 install --user ~/Downloads/PyGUI-2.5.4.tar.gz
```
### pip install from remote archive
```sh
PACKAGE_NAME=electrum
sudo -H pip3 install https://download.electrum.org/4.1.2/Electrum-4.1.2.tar.gz#egg=${PACKAGE_NAME}[fast]
```

### setup.py
#### install
```sh
python setup.py install
```
#### uninstall
```sh
python setup.py install --record list_of_files.txt
cat list_of_files.txt | xargs sudo rm -rf
```

### list of packages
```sh
pip list
```

### list of all installed libraries, installed modules
```sh
pip freeze
```
```python
import sys
sys.modules
```
### path to library path to import package
```python
mapr.ojai.storage.__path__
dir(mapr.ojai.storage)
```

### list of all folders with source code, installed packages
```python
# path to folder with all packages
import sys
print(sys.prefix)
```
for easy_install, pip
```python
import site
print(site.getsitepackages())
```

### using pip from interpreter ( install wheels package)
```python
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
cat /etc/pip.conf 
```
```properties
[global]
index-url = https://cc-artifactory.myserver.net/artifactory/api/pypi/adp-pypi-virtual/simple
```

### install certain version of artifact
```
pip install tornado==2.1.1
```

### [virtual environment manager](https://github.com/pypa/pipenv)
```sh
pip install pipenv
```

### create virtual environment, dedicated env
```sh
pip install virtualenv
```
```sh
# create 
#python3 -m virtualenv venv
virtualenv venv

# activate
source venv/bin/activate

# commands will be executed into virtual env
pip install wheel 
python3 

# check virtual env
echo $VIRTUAL_ENV

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
export PYTHONPATH=$PYTHONPATH:"/home/ubuntu/.local/lib/python3.8/site-packages/gunicorn"
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

for transitive dependency:
* constraints.txt file should be considered  `pip install -c constraints.txt`
* poetry ( under the hood uses pip )


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

### script execution ModuleNotFoundError
when you execute your own project locally
```text
ModuleNotFoundError: No module named 'list_comparator'
```
```sh
echo $PYTHONPATH
export PYTHONPATH=$PYTHONPATH:"/home/projects/wondersign/integration-prototype"

/home/projects/integration-prototype/list-comparator/venv/bin/python /home/projects/integration-prototype/list-comparator/list_comparator/data_api/tools/data_api_reader.py
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
# execute cmd inline with arguments -c with args, string url encode
python -c 'import urllib.parse;import sys;print(urllib.parse.quote(sys.argv[1]))' "Hallo Björn@Python"
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

### special methods
```
__call__  <class instance>()
```

### special file name folder execution main execute main
```
mkdir ./my_folder
touch ./my_folder/__main__.py
python3 ./my_folder
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
# breakpoint()
```
for ipdb:
```python
# python3 -m pip install --user ipdb
__import__('ipdb').set_trace(context=21)
```

execute python app in debug 
```sh
python3 -m pdb myscript.py
python3 -m pdb myscript.pex arg1 arg2 
pdb myscript.py
```

debug commands
```
b my_python_file.py:65
cont
step
next
p x+y
until 99999
continues
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
tox.ini
```
commands = pex . -c download_symbolic_link_creation.py --disable-cache -i {env:PIP_INDEX_URL} -r requirements.txt -o {env:PEX_OUTPUT_PATH} --python-shebang="/usr/bin/env python3.8"
```
setup.py
```py
from setuptools import setup

setup(
    name='download_python_app',
    version='0.0.1',
    scripts=['download_python_app/download_symbolic_link_creation.py']
)
```

## JIT
### just in time compiler pypy
```sh
pip install pypy
pypy app.py
# or
#!/usr/bin/env pypy
```

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
# installation
sudo apt-get remove black && python3 -m pip install black==20.8b1
# execution
black --line-length 100
python -m black {file of directory}
```

### black with IDEA, black with PyCharm
* install FileWatcher plugin
* Tool to Run on Changes: Program: ~/.locl/bin/black
* Tool to Run on Changes: Arguments: $FilePath$

### [black playground](https://black.vercel.app/)

## Test coverage
```sh
# cli tool
pip3 install tox 
# plugin for PyCharm: pytest-cov
```

## run tests
```sh
python -m unittest airflow_shopify.shopify.test_shopify_common
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

# GUI Visual Elements User Interface
* PySimpleGUI
	```sh
	sudo apt-get install python3-tk
	pip3 install PySimpleGUI
	```
* PyGUI
* Wax 
* LibAvg 
* PyGame
* PyQT5
	```sh
	pip install pyqt5
	```
* Python Tkinter
* PySide 2
* Kivy
	```sh
	pip install docutils pygments pypiwin32 kivy.deps.sd12 kivy.deps.glew
	pip install kivy
	```
* wxPython
	```sh
	pip install wxPython
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

# Database
## PostgreSQL
```sh
pip3 install psycopg2-binary
# need to execute: yum install rpm-build
pip3 install -U pip
```
