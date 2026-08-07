# notifications

[![Go Reference](https://pkg.go.dev/badge/github.com/go-freedesktop/notifications.svg)](https://pkg.go.dev/github.com/go-freedesktop/notifications)

Implements the freedesktop.org
[**Desktop Notifications Specification**](https://specifications.freedesktop.org/notification-spec/latest/) —
the **service** side of `org.freedesktop.Notifications`, the D-Bus interface desktop
applications call (through `notify-send`, libnotify, `GLib.Notification`, …) to post
notification bubbles. It decodes those `Notify` calls and hands them to a renderer such
as the [go-widgets](https://github.com/go-widgets/toolkit) `Toast`. Pure Go, `CGO_ENABLED=0`.

- Claim the well-known name `org.freedesktop.Notifications` on the session bus.
- Decode `Notify` / `CloseNotification` / `GetCapabilities` / `GetServerInformation` calls.
- Drive a toast daemon from your render loop; emit `NotificationClosed` / `ActionInvoked` signals back.

## Install

```sh
go get github.com/go-freedesktop/notifications
```

## Usage

```go
conn, _ := dbus.ConnectSessionBus()
daemon := toast.NewDaemon(nil, toolkit.DefaultDark(), myIconLookup)
server := notifications.NewServer(conn, daemon)
daemon.SetEmitter(server)
if err := server.Export(); err != nil { // claims org.freedesktop.Notifications
    log.Fatal(err)
}
// ... call daemon.Tick() from your render loop and draw daemon.Toasts().
```

The full API is on [pkg.go.dev](https://pkg.go.dev/github.com/go-freedesktop/notifications).
Source: [github.com/go-freedesktop/notifications](https://github.com/go-freedesktop/notifications).
