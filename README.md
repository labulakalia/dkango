# WIP: Dokan binding fo Go

Dokan: https://ja.wikipedia.org/wiki/Dokan

- Depends only on the Dokan library (No CGO)
- fs.FS support

## Usage

### Install Dokan library

```sh
winget install dokan-dev.Dokany
```

### examples/simple/simple.go

```go
package main

import (
	"os"
	"github.com/binzume/dkango"
)

func main() {
	dkango.Init()
	defer dkango.Shutdown()

	mount, _ := dkango.MountFS("X:", os.DirFS("."), nil)
	defer mount.Close()

	// Block forever
	select {}
}
```

### need writable FS? (WIP)

```go
type WritableFS interface {
	fs.FS
	OpenWriter(name string) (io.WriteCloser, error)
	Truncate(name string, size int64) error
}
```

## License

MIT