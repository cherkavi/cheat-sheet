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

## set local tag
```yaml
!TAGNAME
```

## anchor
```yaml
*ANCHORNAME
```

## set URI
```yaml
%TAG!uri:
```
