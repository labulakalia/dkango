# Dokan bindings for Go
[![Build Status](https://github.com/binzume/dkango/actions/workflows/test.yaml/badge.svg)](https://github.com/binzume/dkango/actions)
[![Go Reference](https://pkg.go.dev/badge/github.com/binzume/dkango.svg)](https://pkg.go.dev/github.com/binzume/dkango)
[![license](https://img.shields.io/badge/license-MIT-4183c4.svg)](https://github.com/binzume/dkango/blob/master/LICENSE)

Dokan: https://ja.wikipedia.org/wiki/Dokan

- Depends only on the Dokan library (No cgo)
- fs.FS interface support
- Implemented as a backend for https://github.com/binzume/fsmount

## Usage

### Install Dokan library

```sh
winget install dokan-dev.Dokany
```


### API

The only recommended interface is  `dkango.MountFS(mountPoint, fsys, options)`. You can add your file system to the OS with a few lines of code.

[dokan](https://pkg.go.dev/github.com/binzume/dkango/dokan) package provides the low-level API for Dokan, but it still does not provide a stable interface. small breaking changes will be made in the future without any notice.

### examples/simple/simple.go

```go
package main

import (
	"os"
	"github.com/binzume/dkango"
)

func main() {
	mount, _ := dkango.MountFS("X:", os.DirFS("."), nil)
	defer mount.Close()

	// Block forever
	select {}
}
```

### How to create a writable FS?

See examples/writable/writable.go

```
go run ./examples/writable testdir R:
```

```go
type OpenWriterFS interface {
	fs.FS
	OpenWriter(name string, flag int) (io.WriteCloser, error)
}
```

Other interfaces such as RemoveFS, MkdirFS, RenameFS... are also available.

### Cross platform?

See https://github.com/binzume/fsmount

## License

MIT
