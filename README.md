# Edith Flatpak repository

A Flatpak remote for Edith, a GTK4 client for editing files that live on a
remote server over SFTP and FTP.

- Project page: <https://singular.de/apps/edith/>
- Source and issues: <https://github.com/mkay/edith>

Edith is not on Flathub, so this repo is how you install the Flatpak and keep it
updated.

## Install

```sh
flatpak remote-add --user edith https://mkay.github.io/edith-flatpak/edith.flatpakrepo
flatpak install edith de.singular.edith
```

Updates then arrive with `flatpak update` like any other app.

The GNOME runtime Edith builds against is **not** hosted here — it comes from
Flathub, so you need that remote too:

```sh
flatpak remote-add --if-not-exists --user flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

## What is in here

`repo/` is a static [OSTree](https://ostreedev.github.io/ostree/) repository in
`archive-z2` mode: every file is stored once under the hash of its contents, so
successive releases share everything that did not change, and a client fetches
only what it is missing. Nothing but a plain HTTP server is required to serve it.

Commits are GPG-signed; the public key is embedded in `edith.flatpakrepo`, so
`flatpak` verifies every update against it.

Debug symbols are not published — they roughly double the repository for
something no user of a binary remote consumes. Build from source if you need
them.

## Publishing

`build-aux/flatpak/publish-repo.sh` in the main Edith repository writes into
this checkout, but only these two paths — everything else here is hand-written
and survives a publish untouched:

| Path | |
| --- | --- |
| `repo/` | deleted and recopied on every publish |
| `edith.flatpakrepo` | regenerated on every publish, so the embedded key cannot drift from the key the repo was signed with |

`index.html`, this README and the images are safe to edit by hand.
