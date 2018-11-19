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
easy_install.main( ["-U","termcolor"] )
```

## package manager pip
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

### install certain version of artifact
```
pip install tornado==2.1.1
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

### update package
```
pip install tornado --update
```

### list of all installed libraries, installed modules
```
pip freeze
```
```
import sys
sys.modules
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

### tornado inline GET parameters
```
app = tornado.web.Application([(r"/file/([a-zA-Z\-0-9\.:,/_]+)", FileHandler, dict(folder=folder)),])

class FileHandler(tornado.web.RequestHandler):
    def get(self, relative_path):
        print(relative_path)
```
### tornado upload file by chank
```
import tornado.web
import tornado.ioloop

MB = 1024 * 1024
GB = 1024 * MB
TB = 1024 * GB

MAX_STREAMED_SIZE = 1 * GB


@tornado.web.stream_request_body
class MainHandler(tornado.web.RequestHandler):

    def initialize(self):
        print("start upload")

    def prepare(self):
        self.f = open("test.png", "wb")
        self.request.connection.set_max_body_size(MAX_STREAMED_SIZE)

    def post(self):
        print("upload completed")
        self.f.close()

    def put(self):
        print("upload completed")
        self.f.close()

    def data_received(self, data):
        self.f.write(data)


if __name__ == "__main__":
    application = tornado.web.Application([
        (r"/", MainHandler),
    ])
    application.listen(7777)
tornado.ioloop.IOLoop.instance().start()
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
## IDEA
### standard modules not found 
```
Have you set up a python interpreter facet?
Open Project Structure CTRL+ALT+SHIFT+S

Project settings -> Facets -> expand Python click on child -> Python Interpreter

Then:
Project settings -> Modules -> Expand module -> Python -> Dependencies -> select Python module SDK
```
