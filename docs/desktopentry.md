# desktopentry

[![Go Reference](https://pkg.go.dev/badge/github.com/go-freedesktop/desktopentry.svg)](https://pkg.go.dev/github.com/go-freedesktop/desktopentry)

Implements the freedesktop.org
[**Desktop Entry Specification**](https://specifications.freedesktop.org/desktop-entry-spec/latest/) —
the layer a dock, application menu or Spotlight-style finder needs: **enumerate the
installed applications and expand their launch commands.** Pure Go, `CGO_ENABLED=0`.

- Scan the standard XDG application directories for visible `.desktop` entries.
- Parse an entry: `Name`, `Icon`, `Exec`, `TryExec`, categories, and actions.
- Expand `Exec=` into an `argv`, resolving field codes (`%f`, `%U`, `%i`, …) against the files you pass.

## Install

```sh
go get github.com/go-freedesktop/desktopentry
```

## Usage

```go
package main

import (
	"fmt"
	"os/exec"

	"github.com/go-freedesktop/desktopentry"
)

func main() {
	// The launcher / Spotlight index: every visible installed app.
	for _, e := range desktopentry.Scan() {
		fmt.Printf("%-30s %s  (icon: %s)\n", e.ID, e.Name, e.Icon)
	}

	// Launch one, opening a file.
	e, _ := desktopentry.ParseFile("/usr/share/applications/org.gnome.gedit.desktop")
	argv, err := e.ExpandExec([]string{"/tmp/notes.txt"}, "")
	if err != nil {
		panic(err)
	}
	_ = exec.Command(argv[0], argv[1:]...).Start()

	// Offer an entry's actions ("New Window", …) — already parsed.
	for _, a := range e.Actions {
		fmt.Println("action:", a.Name, "→", a.Exec)
	}
}
```

The full API is on [pkg.go.dev](https://pkg.go.dev/github.com/go-freedesktop/desktopentry).
Source: [github.com/go-freedesktop/desktopentry](https://github.com/go-freedesktop/desktopentry).
