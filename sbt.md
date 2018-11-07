## create new project, Giter8 template 
```
sbt new sbt/scala-seed.g8
```
or
```
sbt new scala/hello-world.g8
```
or 
```
# https://github.com/MrPowers/spark-sbt.g8
MrPowers/spark-sbt.g8
```

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

## create uber jar
* project/assembly.sbt
```
addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.7")
```
* build.sbt
```
mainClass := Some("com.bmw.ad.solr.ConnectionCheck")
test in assembly := {}
```
* command to build uber jar target/scala-{version}/{project-name}.jar
```
sbt assembly
```

## enter into interactive mode
```
sbt
```

## execute command from interactive mode via direct sbt call
```
sbt "runMain com.cherkashyn.vitalii.finch.Finchwebapp"
```


## default local lib storage, local repository
```
$HOME/.ivy2/cache/{path to library}
```
