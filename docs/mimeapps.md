# mimeapps

[![Go Reference](https://pkg.go.dev/badge/github.com/go-freedesktop/mimeapps.svg)](https://pkg.go.dev/github.com/go-freedesktop/mimeapps)

Implements the freedesktop.org
[**Association between MIME types and applications**](https://specifications.freedesktop.org/mime-apps-spec/latest/)
specification (`mimeapps.list`) — given a MIME type, **which application opens it by
default, and what is the ordered list of candidates?** With round-trippable edits, it is
the engine behind an *"Open With…"* menu. Pure Go, `CGO_ENABLED=0`, building on
[`desktopentry`](desktopentry.md).

- Merge the `mimeapps.list` layers per the XDG environment and `$XDG_CURRENT_DESKTOP`.
- Resolve the default handler and the ordered candidate list for a type.
- Persist a user's default choice back to `mimeapps.list`.

## Install

```sh
go get github.com/go-freedesktop/mimeapps
```

## Usage

```go
package main

import (
	"fmt"

	"github.com/go-freedesktop/mimeapps"
)

func main() {
	r := mimeapps.Load() // standard XDG environment + $XDG_CURRENT_DESKTOP

	// The default handler for a MIME type.
	if app, err := r.DefaultApp("text/html"); err == nil {
		fmt.Println("default:", app.Name, "→", app.Exec)
	}

	// The ordered "Open With…" menu.
	for _, app := range r.Candidates("image/png") {
		fmt.Println("candidate:", app.ID, app.Name)
	}

	// Make an application the user's default (persisted to mimeapps.list).
	_ = r.SetDefault("text/html", "org.mozilla.firefox.desktop")
}
```

The full API is on [pkg.go.dev](https://pkg.go.dev/github.com/go-freedesktop/mimeapps).
Source: [github.com/go-freedesktop/mimeapps](https://github.com/go-freedesktop/mimeapps).
