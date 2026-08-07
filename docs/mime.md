# mime

[![Go Reference](https://pkg.go.dev/badge/github.com/go-freedesktop/mime.svg)](https://pkg.go.dev/github.com/go-freedesktop/mime)

Implements the freedesktop.org
[**Shared MIME-info Database**](https://specifications.freedesktop.org/shared-mime-info-spec/latest/) —
answer the one question *"what type is this file?"* From a file's name, its content
(magic), or both, it returns a canonical MIME type against the on-disk shared database.
Pure Go, `CGO_ENABLED=0`.

- Fast name-only classification (the path a listing view uses).
- Content sniffing (magic) and combined name + content resolution.
- Type relationships: alias resolution and subclass (`is-a`) checks.

## Install

```sh
go get github.com/go-freedesktop/mime
```

## Usage

```go
package main

import (
	"fmt"
	"os"

	"github.com/go-freedesktop/mime"
)

func main() {
	// Name only — the fast path a listing view uses.
	fmt.Println(mime.TypeByName("report.pdf")) // application/pdf

	// Name + a content sniff — what "Get Info" / "Open With" should use.
	f, _ := os.Open("/tmp/download")
	head := make([]byte, 256)
	n, _ := f.Read(head)
	fmt.Println(mime.TypeByNameAndContent("download", head[:n]))

	// Relationships.
	fmt.Println(mime.Unalias("application/x-gzip"))               // application/gzip
	fmt.Println(mime.IsSubclassOf("image/svg+xml", "text/plain")) // true
}
```

The full API is on [pkg.go.dev](https://pkg.go.dev/github.com/go-freedesktop/mime).
Source: [github.com/go-freedesktop/mime](https://github.com/go-freedesktop/mime).
