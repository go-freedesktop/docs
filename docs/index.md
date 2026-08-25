# go-freedesktop documentation

**Pure-Go (`CGO_ENABLED=0`) implementations of the [freedesktop.org](https://www.freedesktop.org/)
desktop-integration specifications.** No cgo, no D-Bus C library, no external tools.

`go-freedesktop` is a family of small, independent Go modules that implement the
specifications a launcher, file manager or desktop shell relies on — enumerating
installed applications, resolving icons, classifying files by MIME type, choosing
which application opens a type, building the category menu, and serving desktop
notifications. Each spec is one small module, so a caller pulls in only what it needs. Two of the
modules are not specifications at all: `x11` is the X11 protocol's byte layer, and
`screencast` is the capture built on it.

## Modules

| Module | freedesktop specification | What it gives you |
| --- | --- | --- |
| [`desktopentry`](desktopentry.md) | [Desktop Entry](https://specifications.freedesktop.org/desktop-entry-spec/latest/) | Enumerate installed applications and expand their `Exec=` launch commands. |
| [`icontheme`](icontheme.md) | [Icon Theme](https://specifications.freedesktop.org/icon-theme/latest/) | Resolve an icon *name* to a file *path* at a target size and scale. |
| [`mime`](mime.md) | [Shared MIME-info Database](https://specifications.freedesktop.org/shared-mime-info-spec/latest/) | Resolve a canonical MIME type from a file's name, content, or both. |
| [`mimeapps`](mimeapps.md) | [MIME Applications Associations](https://specifications.freedesktop.org/mime-apps-spec/latest/) | Default application + ordered candidates for a MIME type. |
| [`menu`](menu.md) | [Desktop Menu](https://specifications.freedesktop.org/menu-spec/latest/) | Turn `applications.menu` XML into a categorized tree of app entries. |
| [`notifications`](notifications.md) | [Desktop Notifications](https://specifications.freedesktop.org/notification-spec/latest/) | The `org.freedesktop.Notifications` D-Bus service side. |
| [`secretservice`](secretservice.md) | [Secret Service](https://specifications.freedesktop.org/secret-service-spec/latest/) | Store and read secrets where the desktop already keeps them (GNOME Keyring, KWallet). |
| [`screencast`](screencast.md) | *(not a spec — X11)* | Capture the pixels of displays and windows. Wayland is not implemented, and says so. |
| [`x11`](x11.md) | *(not a spec — the protocol itself)* | The byte layer under `screencast` and `go-widgets/window`: wire codec, Xauthority, setup, MIT-SHM, `SCM_RIGHTS`. |

## Design

- **Pure Go, `CGO_ENABLED=0`** — one portable module per spec; cross-compiles to a static binary.
- **Reuse, don't reinvent** — standard XDG base-directory resolution is shared, not re-implemented; later waves build on earlier ones (`mimeapps` and `menu` stand on `desktopentry`).
- **100% test coverage** is the bar, error branches included, on native amd64/arm64 plus emulated 64-bit targets.
- **BSD-3-Clause** throughout.

## Links

- 🌐 Site — <https://go-freedesktop.github.io/>
- 💻 GitHub — <https://github.com/go-freedesktop>
- 🎨 Brand — <https://github.com/go-freedesktop/brand>
