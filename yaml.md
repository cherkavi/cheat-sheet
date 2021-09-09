# Yet Another Markup Language
* [specification](https://yaml.org/spec/1.2/spec.html)

## indentation
2 spaces

## data types
* string
* number
* boolean
* list
* map
* <empty value>
  
```yaml
host: ger-43
datacenter:
location: Germany
cab: "13"
cab_unit: 3

# block-style
list_of_strings:
- one
- two
- three
# flow-style
another_list_of_strings: [one,two,three]

# block-style
map_example:
  - element_1: one
  - element_2: two
# flow-style
map_example2: {element_1: one , element_2: two }
```
more comples example - list of maps
```yaml
credentials:
  - name: 'user1'
    password: 'pass1'
  - name: 'user2'
    password: 'pass2'
```


## comment
```yaml
# this is comment in yaml
user_name: cherkavi # comment after the value
user_password: "my#secret#password"
```


## multiline
* one line string ( removing new line marker )
```yaml
comments: >
Attention, high I/O
since 2019-10-01.
Fix in progress.
```

* each line of text - with new line marker ( new line preserving )
```yaml
downtime_sch: |
2019-10-05 - kernel upgrade
2019-02-02 - security fix
```

## multi documents
```yaml
---
name: first document

---
name: second document

# not necessary marker ...
...
---
name: third document
```
place of metadata, header
```yaml
# this is metadata storage place
---
name: first document
```

## anchor, reference  
& - anchor
* - reference to anchor
simple reference
```yaml
---
host: my_host_01
description: &AUTO_GENERATE standard host without specific roles
---
host: my_host_02
description: *AUTO_GENERATE
```
reference to map
```yaml
users:
  - name: a1 &user_a1    # define anchor "user_a1"
    manager:
  - name: a2
    manager: *user_a1    # reference to anchor "user_a1"
```
example with collection
```yaml
---
host: my_host_01
roles: &special_roles
  - ssh_server
  - apache_server
---
host: my_host_02
roles: *special_roles
```
another yaml example, anchor reference
```yaml
base: &base
    name: Everyone has same name

foo: &foo
    <<: *base
    age: 10

bar: &bar
    <<: *base
    age: 20  
```

## TAG
### set data type for value
* seq
* map
* str
* int
* float
* null
* binary
* omap ( Ordered map )
* set ( unordered set )
```yaml
name: Vitalii
id_1: 475123 !!str
id_2: !!str 475123 
```

### assign external URI to tag ( external reference )
should be defined inside metadata
```yaml
%TAG ! tag:hostdata:de_muc
---
# reference id is: DE_MUC
location: !DE_MUC Cosimabad   
```
### local tag ( reference inside document )
