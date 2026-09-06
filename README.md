# garm-provider-incus-bin

AUR package that installs
[garm-provider-incus](https://github.com/cloudbase/garm-provider-incus) from
the official upstream binary release.

garm-provider-incus is the external compute provider that lets
[GARM](https://github.com/cloudbase/garm) (GitHub Actions Runner Manager)
spawn ephemeral GitHub Actions runners as Incus containers or virtual
machines.

## What's included

- **Binary:** `/opt/garm/providers.d/garm-provider-incus` (statically
  linked, straight from the upstream release tarballs, for both `x86_64`
  and `aarch64`) — installed into GARM's default external provider
  directory, which is owned by the `garm-bin` package

## Install

```bash
yay -S garm-provider-incus-bin
```

Or manually:

```bash
git clone https://aur.archlinux.org/garm-provider-incus-bin.git
cd garm-provider-incus-bin
makepkg -si
```

## Setup

Register the provider in `/etc/garm/config.toml`:

```toml
[[provider]]
  name = "incus"
  provider_type = "external"
  [provider.external]
    provider_executable = "/opt/garm/providers.d/garm-provider-incus"
    config_file = "/etc/garm/garm-provider-incus.toml"
```

Create the provider's own config file — the upstream
[sample config](https://github.com/cloudbase/garm-provider-incus/blob/main/testdata/garm-provider-incus.toml)
is a good starting point.

The user GARM runs as needs access to the Incus socket, e.g.:

```bash
echo "m garm incus-admin" | sudo tee /etc/sysusers.d/garm-incus.conf
sudo systemd-sysusers
```

## Automatic updates

A GitHub Actions workflow checks daily for new garm-provider-incus releases
and pushes updates to the AUR automatically.

## License

The packaging files in this repository are provided under
[Apache-2.0](LICENSE.md). garm-provider-incus itself is licensed under
Apache-2.0 by the
[upstream project](https://github.com/cloudbase/garm-provider-incus).
