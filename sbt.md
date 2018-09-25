## clean 
```
sbt clean clean-files
find . -name target -type d -exec rm -rf {} \;
```

## compile
```
sbt clean compile
```

## continue to test after building
```
sbt ~testQuick
```

## create jar
```
sbt package
```

## specify main-class, mainClass for manifest
```
mainClass := Some("com.cherkashyn.solr.ConnectionCheck")
```
