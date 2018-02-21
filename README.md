go-buck-generator
=================

`go-buck-generator` is a small bash script to automatically generate BUCK target files for third party go packages in a buck project.  

Sometimes it's very tedious to hand-write a BUCK file for every external go project that your code uses - sometimes there are a lot of dependencies, other times there are many source files that are included or excluded with [go build tags](https://golang.org/pkg/go/build/#hdr-Build_Constraints) which Buck unfortunately doesn't support.

`go-buck-generator` automates this tediousness. It will generate a BUCK target file for every package it finds in $GOPATH, automatically adding source files (generating separate go\_library rules for each valid combination of GOOS/GOARCH based on build tags) and dependencies.

Usage
-----

For a buck project with a root of `~/my-project`, third party Go code stored under `~/my-project/third_party/go`  
`env GOPATH=~/my-project/third_party/go go-buck-generator`  
This will create `BUCK` files for each package under `~/my-project/third_party/go/src`
