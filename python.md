### find vs index ( string find, string index )
```
find - return -1 if not found
index - throw exception if not found
```

### install pip
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
```

### upgrade pip
```
pip install -U pip
```

### install certain version of artifact
```
pip install tornado==2.1.1
```

### install from git
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

### list of all installed libraries
```
pip freeze
```

### using pip from interpreter ( install wheels package)
```
import pip
pip.main("install", "wheels")
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

### execute string as commands
```
a=10
eval(" print(a)")
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
