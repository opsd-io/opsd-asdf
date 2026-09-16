# Contributing

## Pull Requests And Commits

Use the following format for the pull request title and every commit:

~~~text
type(scope): short imperative description
~~~

Allowed scopes are asdf, ci, docs, and release, for example
fix(asdf): verify checksum before install. The Conventional Commits check is
required; Release Please uses these messages for release notes.

## Local Validation

Check all plugin scripts before opening a pull request:

~~~bash
find bin -type f -exec sh -n {} \;
~~~

The GitHub Actions workflow also tests version listing, checksum verification,
download, and installation from a temporary release bundle. Keep the plugin
portable across supported operating systems and architectures. Do not commit
release bundles or credentials.

Changes to the CLI bundle format must be coordinated with opsd-cli and
validated against a matching GitHub Release asset.
