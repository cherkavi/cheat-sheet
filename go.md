# Go lang 

## Issues
### bazel buildfier
#### intention
```sh
go install github.com/bazelbuild/buildtools/buildifier@latest
```
#### error message
```
can't load package: package github.com/bazelbuild/buildtools/buildifier@latest: cannot use path@version syntax in GOPATH mode
```
#### solution
```sh
GOPATH=/home/projects/goroot
go install github.com/bazelbuild/buildtools/buildifier
cd $GOPATH/src/github.com/bazelbuild/buildtools/buildifier
bazel build :all
```
possible (didn't check it) alternative way
```sh
go mod init buildifier
# go mod init .
go mod download repo@version
# go mod download github.com/bazelbuild/buildtools/buildifier@latest
```

#### buildfier
```sh
/home/projects/goroot/bin/buildifier -mode fix {file_path}
# bazel run //bazel/tools/buildifier:fix
```
