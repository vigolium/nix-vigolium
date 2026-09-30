# Vigolium for Nix

Official binary flake for [Vigolium](https://github.com/vigolium/vigolium).
Supports x86_64 and aarch64 on Linux and macOS.

```sh
nix run github:vigolium/nix-vigolium -- version
nix profile install github:vigolium/nix-vigolium
```

Enable Nix flakes and the `nix-command` feature in your Nix configuration.
To pin a version, append its tag, such as `github:vigolium/nix-vigolium/v0.5.1`.
For upgrades, use `nix profile list` to find the profile entry name and then
`nix profile upgrade <entry-name>`.

Linux uses an FHS environment for the dynamically linked helpers embedded in
the release binary. Working user namespaces and bubblewrap are required; some
restricted containers cannot run it. Ubuntu's AppArmor restrictions may require
an administrator to permit user namespaces for Nix's exact bubblewrap executable.
The disposable CI runners apply that scoped profile for testing. Configure `spidering.browser_path` for an
external browser when needed. Chromium is not bundled. Configuration and scan
data stay in the user's normal Vigolium directories.

Use Nix to update the binary. The wrapper disables Vigolium's standalone update
check. This flake is distributed directly from this repository; it is not yet
included in the central nixpkgs package collection.

## Release updates

The **Update release** GitHub Actions workflow checks the latest published stable
release of `vigolium/vigolium` hourly. It downloads and verifies the five upstream
archives against `checksums.txt`, reads license notices from the release tag, and
tests the candidate before committing it. Ordinary upstream commits and
prereleases do not publish a new package. Older versions cannot replace newer
ones, and version tags are never overwritten.

You can also run the workflow manually with an explicit stable tag. Enable
`force` to test the current version again. Failed validation leaves the current
package in place. GitHub may delay scheduled runs, and disables schedules in
public repositories after 60 days without repository activity; re-enable the
workflow in Actions if that happens.

The publishing job uses this repository's automatic, short-lived `GITHUB_TOKEN`
with `contents: write`. No personal access token, password, or custom secret is
required. Download and test jobs have read-only access.

Recipe generation and automation sources are maintained in
[`vigolium/vigolium/build/packaging`](https://github.com/vigolium/vigolium/tree/main/build/packaging).
The `.github/packaging` copy and pinned Nix package set/Scoop installer are updated
deliberately when the packaging tools change. This initial setup originated from
the packaging changes prepared locally; that source link will be available once
those changes are merged upstream.
