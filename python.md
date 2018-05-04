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

### install certain version of artifact
```
pip install tornado==2.1.1
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
pip.main("install", "wheels")
```

### import package by string name
```
target = __import__("data-migration")
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
