# screencast

[![Go Reference](https://pkg.go.dev/badge/github.com/go-freedesktop/screencast.svg)](https://pkg.go.dev/github.com/go-freedesktop/screencast)

Capture the pixels of displays and windows **on Linux**, in pure Go, `CGO_ENABLED=0`,
without shelling out to anything — no `xwd`, no `import`, no `grim`, no `ffmpeg`, and no
Xlib or XCB. It speaks the X11 wire protocol itself through
[`x11`](x11.md), and lets the X server write captured frames straight into shared memory
it handed over with `SCM_RIGHTS`.

It is the Linux sibling of
[`go-macos/screencapture`](https://github.com/go-macos/screencapture) and presents
deliberately the same shape, so one consumer drives both platforms through
near-identical adapters.

## Install

```sh
go get github.com/go-freedesktop/screencast
```

## Usage

```go
content, err := screencast.Shareable(ctx)
d, err := content.MainDisplay()

s, err := screencast.CaptureDisplay(ctx, d, screencast.Options{FPS: 60})
defer s.Close()

for {
    f, fresh := s.Frame()      // borrowed BGRA bytes; no allocation
    if fresh {
        upload(f.Pix, f.Width, f.Height, f.Stride)   // index with Stride
    }
}
```

## Backends, and what is not implemented

| Backend | State |
| --- | --- |
| **X11** | works — MIT-SHM 1.2 with `AttachFd`, core `GetImage` fallback, MIT-MAGIC-COOKIE-1, RANDR 1.5 and XINERAMA, XFIXES cursor, every TrueColor/DirectColor visual at 16, 24 and 32 bpp |
| **Wayland** | **not implemented.** A wlroots compositor exposing `wlr-screencopy-unstable-v1` could be driven the same way; it is not done here. Under Wayland, capture works only through Xwayland |
| **xdg-desktop-portal** | **deliberately not started.** The sanctioned GNOME/KDE route hands back a **PipeWire** stream, and there is no pure-Go PipeWire client here. `Diagnose()` detects that situation and says so, rather than failing obscurely — it only *looks*, and never opens a session or prompts anyone |

## Proof, not assertion

"The frame is not all zeroes" is a weak claim: a capture that read the wrong drawable,
swapped red and blue, or sheared every row would pass it. So the integration suite and
`sccheck -selftest` **paint** — they cover the display with a colour built from the
visual's own channel masks, and then require every captured pixel to be exactly that
colour, in that byte order, at the declared stride, and then require the next colour to
be seen. It passes identically on a 5-6-5 visual, an 8-8-8 one and a 10-10-10 one.

Everything measured so far was measured against `Xvfb` on a VM with **no GPU**. The
repository's README says which figures that qualifies and which it does not.

The full API is on [pkg.go.dev](https://pkg.go.dev/github.com/go-freedesktop/screencast).
Source: [github.com/go-freedesktop/screencast](https://github.com/go-freedesktop/screencast).
