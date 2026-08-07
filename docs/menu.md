# menu

[![Go Reference](https://pkg.go.dev/badge/github.com/go-freedesktop/menu.svg)](https://pkg.go.dev/github.com/go-freedesktop/menu)

Implements the freedesktop.org
[**Desktop Menu Specification**](https://specifications.freedesktop.org/menu-spec/latest/) —
turn the installed applications into the **categorized menu** a launcher shows
(Accessories, Graphics, System, Games, …). It reads the `applications.menu` XML and
produces a resolved tree of directories and app entries. Pure Go, `CGO_ENABLED=0`,
building on [`desktopentry`](desktopentry.md).

- Parse the standard XDG `applications.menu` layout (with `<Include>`/`<Exclude>`/`<Directory>` rules).
- Resolve each category to its directory name/icon and the app entries it contains.
- Walk the tree of submenus for rendering.

## Install

```sh
go get github.com/go-freedesktop/menu
```

## Usage

```go
package main

import (
	"fmt"

	"github.com/go-freedesktop/menu"
)

func main() {
	tree, err := menu.Load() // reads the standard XDG applications.menu
	if err != nil {
		panic(err)
	}
	walk(tree.Root, 0)
}

func walk(m *menu.Menu, depth int) {
	fmt.Printf("%*s%s\n", depth*2, "", m.DirectoryName)
	for _, a := range m.Apps {
		fmt.Printf("%*s- %s  (%s)\n", depth*2+2, "", a.Name, a.Icon)
	}
	for _, sub := range m.Submenus {
		walk(sub, depth+1)
	}
}
```

The full API is on [pkg.go.dev](https://pkg.go.dev/github.com/go-freedesktop/menu).
Source: [github.com/go-freedesktop/menu](https://github.com/go-freedesktop/menu).
