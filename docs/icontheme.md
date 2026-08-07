# icontheme

[![Go Reference](https://pkg.go.dev/badge/github.com/go-freedesktop/icontheme.svg)](https://pkg.go.dev/github.com/go-freedesktop/icontheme)

Implements the freedesktop.org
[**Icon Theme Specification**](https://specifications.freedesktop.org/icon-theme/latest/)
lookup algorithm — resolve an icon **name** (such as the `Icon=` value of a `.desktop`
entry) to a concrete file **path** at a requested pixel size and scale, following theme
inheritance and the `hicolor` fallback. Pure Go, `CGO_ENABLED=0`.

- Load a named theme and its `Inherits` chain, ending at `hicolor`.
- Look up one name, or a list of candidate names, at a target size and HiDPI scale.
- Directory matching honours `Type` (`Fixed`/`Scalable`/`Threshold`), `MinSize`/`MaxSize` and `Scale`.

## Install

```sh
go get github.com/go-freedesktop/icontheme
```

## Usage

```go
package main

import (
	"fmt"

	"github.com/go-freedesktop/icontheme"
)

func main() {
	theme := icontheme.New("Adwaita") // falls back through Inherits, then hicolor

	// Resolve a single name at 48px, scale 1.
	path, err := theme.Lookup("text-editor", 48, 1)
	if err != nil {
		fmt.Println("not found:", err)
		return
	}
	fmt.Println(path) // e.g. /usr/share/icons/Adwaita/48x48/apps/text-editor.png

	// Try a list of candidate names (Icon= value plus fallbacks), HiDPI scale 2.
	path, err = theme.FindIcon([]string{"org.example.App", "application-x-executable"}, 24, 2)
	fmt.Println(path, err)
}
```

The full API is on [pkg.go.dev](https://pkg.go.dev/github.com/go-freedesktop/icontheme).
Source: [github.com/go-freedesktop/icontheme](https://github.com/go-freedesktop/icontheme).
