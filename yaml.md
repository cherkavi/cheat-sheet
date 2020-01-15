# Yet Another Markup Language

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
cab_unit: "3"

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
```


## multiline
* new line preserving
```yaml
comments: >
Attention, high I/O
since 2019-10-01.
Fix in progress.
```

* without new line 
```yaml
downtime_sch: |
2019-10-05 - kernel upgrade
2019-02-02 - security fix
```

## anchor
```yaml
*ANCHORNAME
```

## start of new directive
```yaml
---

```

## indentation
2 spaces


## set URI
```yaml
%TAG!uri:
```

## set local tag
```yaml
!TAGNAME
```
