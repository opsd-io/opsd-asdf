# asdf-opsd

An [asdf](https://asdf-vm.com/) plugin for installing and managing the OPSd
CLI from release bundles published by [`opsd-io/opsd-cli`](https://github.com/opsd-io/opsd-cli/releases).

## What This Repository Contains

This repository contains the asdf plugin contract:

- `bin/list-all` — lists available OPSd CLI versions;
- `bin/latest-stable` — selects the latest stable version;
- `bin/download` — downloads and unpacks a release bundle;
- `bin/install` — installs the unpacked bundle;
- `.github/workflows/opsd-asdf-ci.yml` — validates the plugin and tests local installation.

The OPSd CLI release bundles are published in GitHub Releases of
[`opsd-io/opsd-cli`](https://github.com/opsd-io/opsd-cli). This repository does
not store release bundles.

## Installation

Add the plugin:

```bash
asdf plugin add opsd https://github.com/opsd-io/opsd-asdf.git
```

Install a version:

```bash
asdf install opsd 1.0.0
asdf set --home opsd 1.0.0
opsd version
```

For older asdf versions, use:

```bash
asdf global opsd 1.0.0
```

## Release Artifacts

By default, the plugin reads versions from the tags of `opsd-io/opsd-cli` and
downloads matching assets from its GitHub Release.

Expected artifact naming:

- `opsd_<version>_<os>_<arch>.tar.gz`

Examples:

- `opsd_1.0.0_linux_amd64.tar.gz`
- `opsd_1.0.0_darwin_arm64.tar.gz`

The release version is represented by a tag in the form `vX.Y.Z`, while the
asset name omits the `v` prefix. Each release must also publish a
`checksums.txt` file containing a SHA-256 checksum for every archive.

## Optional Overrides

Normally you do not need to set any of these variables. The default source is
the GitHub Releases page of `opsd-io/opsd-cli`.

The source can be overridden for local testing or an alternate distribution
endpoint:

- `OPSD_RELEASE_GIT_URL` — repository used by `list-all`;
- `OPSD_RELEASE_BASE_URL` — directory containing release assets;
- `OPSD_RELEASE_OS` — operating system used when selecting an asset;
- `OPSD_RELEASE_ARCH` — architecture used when selecting an asset.

Example:

```bash
# Use a specific GitHub Release instead of the default URL generated from the
# version being installed.
export OPSD_RELEASE_BASE_URL="https://github.com/opsd-io/opsd-cli/releases/download/v1.0.0"
```

The configured URL must contain both the selected archive and
`checksums.txt`. For normal installations, leave `OPSD_RELEASE_BASE_URL`
unset.

## Local Testing

The plugin scripts can be checked locally without publishing a release:

```bash
find bin -type f -exec sh -n {} \;
```

The GitHub Actions workflow also creates a temporary release bundle and tests
version listing, checksum verification, and the complete download and
installation flow. It runs for pull requests and pushes to `main`.

To test the plugin from a local checkout:

```bash
asdf plugin add opsd /path/to/opsd-asdf
asdf install opsd 1.0.0
asdf set --home opsd 1.0.0
opsd version
```

## Reporting Issues

Report problems in the repository where they occur:

- [ASDF plugin issues](https://github.com/opsd-io/opsd-asdf/issues)
- [OPSd CLI issues](https://github.com/opsd-io/opsd-cli/issues)
