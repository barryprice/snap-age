# age (snap)

A Snap package of [age](https://age-encryption.org/) (GitHub:
[FiloSottile/age](https://github.com/FiloSottile/age)) — a simple, modern and
secure file encryption tool, available from the Snap Store here:

https://snapcraft.io/age-bp

> **Community / unofficial.** This is an unofficial, community-maintained snap.
> The snap `name` is `age-bp`; the `title` is `age` so the software is
> recognised by its real name. 

## What's in the snap

The snap installs three binaries from upstream, each with a suggested alias:

| Binary        | Alias         | Purpose                                    |
| ------------- | ------------- | ------------------------------------------ |
| `bin/age`     | `age`         | Encrypt / decrypt files                    |
| `bin/age-keygen` | `age-keygen` | Generate identity keys                  |
| `bin/age-inspect` | `age-inspect` | Inspect age-encrypted files            |

To enable those aliases, after installing:

  ```sh
  sudo snap alias age-bp.age age
  ```

  ```sh
  sudo snap alias age-bp.age-inspect age-inspect
  ```

  ```sh
  sudo snap alias age-bp.age-keygen age-keygen
  ```

The binaries are built statically (`CGO_ENABLED=0`) from the source of the
[matching upstream release](https://github.com/FiloSottile/age/releases). The
snap's version tracks the latest upstream release — the build recipe resolves
the highest `vX.Y.Z` tag from upstream, so a new release is picked up
automatically on the next snap build.

## Installing

On a snap that has been promoted to `stable`:

```sh
snap install age-bp
```

From the `edge` track:

```sh
snap install age-bp --edge
```

### Interfaces / filesystem access

The snap runs under **strict confinement** and declares the following plugs on
each app:

- `home` — read/write access to `$HOME`. This is **auto-connected** on
  install, and is what lets you encrypt/decrypt files under your home
  directory and use keys/identities from `~/.config/age`.
- `removable-media` — access to mounted removable media (`/media`, `/mnt`).
  This is **not** auto-connected; enable it explicitly if you need it:

  ```sh
  sudo snap connect age-bp:removable-media
  ```

> **Note on paths.** Strict confinement limits access to paths outside
> `$HOME`, the snap's own mount points, and `/media`/`/mnt`. If you want to
> encrypt/decrypt files on other mounts (e.g. network shares or `/tmp`), you
> may need to mount them somewhere under the accessible paths, or request an
> additional interface.

## Building locally

Requires `snapcraft` (install with `sudo snap install snapcraft --classic`):

```sh
snapcraft
```

This builds `age-bp_<version>_<arch>.snap` in the project root. You can also
run a local lint:

```sh
snapcraft lint
```

## Continuous integration

[`.github/workflows/snap.yml`](.github/workflows/snap.yml) is CI-only:

- Builds the snap on every push / pull request, and uploads the `.snap` as an
  artifact.
- Runs `snapcraft lint` against the built snap (best-effort by default).

This workflow is a **merge gate only** — it never publishes. Building **and**
publishing to `latest/edge` is handled by snapcraft.io, which schedules its
own builds of this snap and pushes them to the `edge` track automatically.
Promoting to a `stable` track remains a manual decision (via the Snap Store
dashboard). This repo is intentionally never tagged — the upstream `age` repo
is where the release tags live.

## License

The packaged software (age) is licensed under the BSD 3-Clause license, as
recorded in `snap/snapcraft.yaml`.
