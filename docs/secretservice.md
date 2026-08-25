# secretservice

[![Go Reference](https://pkg.go.dev/badge/github.com/go-freedesktop/secretservice.svg)](https://pkg.go.dev/github.com/go-freedesktop/secretservice)

Client for the freedesktop
[**Secret Service API**](https://specifications.freedesktop.org/secret-service-spec/latest/)
(`org.freedesktop.Secret.Service`) over D-Bus — the interface GNOME Keyring and KWallet
implement, and therefore the way a Linux program stores a secret where the desktop
already keeps them. Pure Go, `CGO_ENABLED=0`, over
[godbus](https://github.com/godbus/dbus).

It is the Linux half of [`go-keyring`](https://github.com/go-keyring), whose other halves
are [`go-macos/keychain`](https://github.com/go-macos/keychain) and the Windows
Credential Manager.

## Install

```sh
go get github.com/go-freedesktop/secretservice
```

The full API is on [pkg.go.dev](https://pkg.go.dev/github.com/go-freedesktop/secretservice).
Source: [github.com/go-freedesktop/secretservice](https://github.com/go-freedesktop/secretservice).
