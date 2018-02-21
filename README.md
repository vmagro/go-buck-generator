go-buck-generator
=================

Note: this tool is still a little rough around the edges, see the TODO below  

`go-buck-generator` is a small bash script to automatically generate BUCK target files for third party go packages in a buck project.  

Sometimes it's very tedious to hand-write a BUCK file for every external go project that your code uses - sometimes there are a lot of dependencies, other times there are many source files that are included or excluded with [go build tags](https://golang.org/pkg/go/build/#hdr-Build_Constraints) which Buck unfortunately doesn't support.

`go-buck-generator` automates this tediousness. It will look at all packages in $GOPATH, automatically adding source files (in the future generating separate go\_library rules for each valid combination of GOOS/GOARCH based on build tags) and dependencies.

Usage
-----

For a buck project with a root of `~/my-project`, third party Go code stored under `~/my-project/third_party/go`  
`env GOPATH=~/my-project/third_party/go go-buck-generator`  
This will create `BUCK` files for each package under `~/my-project/third_party/go/src`

Todo
----

This tool is still a little rought around the edges, some (major?) things are unsupported as of now  

- better error messages when a dependency of a package is missing - `buck` will complain but ideally this tool would too
- cgo: cgo is currently ignored, packages that rely on `import "C"` will probably not work
- generate `go_library` rules for all valid [GOOS/GOARCH](https://golang.org/doc/install/source#environment) combinations
  - maybe wrapped in `os.getenv` checks to use the right version for the host platform
  - `buck` seems to have support for different architectures but seems undocumented (when building it shows something like `#linux_amd64` after the target name)
- it's somewhat annoying to have to have another subdirectory `src` but with the way the `go` tool works, this seems to be necessary (see $GOPATH structure)
