# Contributing to mt32-pi

This is a community-maintained fork of [dwhinham/mt32-pi][upstream]. All credit
for the original work goes to Dale Whinham. Contributions are welcome.

Everything written into this repository is English: commit subjects and bodies,
pull request titles and bodies, branch names and changelog entries.

## Branches

Cut every branch from an up-to-date `main` and name it `<type>/<short-summary>`,
using the same type you intend for the commit, for example `fix/lcd-wake` or
`ci/pin-actions`.

Check what `HEAD` is on before branching. A branch cut from another open branch
by mistake carries that branch's work into the wrong pull request.

## Commits and pull request titles

Commits follow [Conventional Commits][cc]. Permitted types are `feat`, `fix`,
`docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore` and
`revert`. The scope is optional and lower case. A breaking change is marked with
`!` after the type and `BREAKING CHANGE:` in the body. Subject lines are at most
100 characters.

The pull request title matters most. Merges are squash merges and the title
becomes the subject line on `main`, which is what determines the next version
and the changelog rubric. A title outside the scheme produces a wrong version or
a missing changelog entry.

One pull request carries changes of one kind. A `feat` title files everything it
contains under Features, including a fix that travelled with it, where nobody
will look for it later. Split the work and merge the parts one after the other.

## Building locally

The build needs the Arm GNU toolchain for `aarch64-none-elf` on the `PATH`.

```
make submodules
make -j BOARD=pi3-64
```

Valid board targets are `pi3-64`, `pi4-64` and `pi5`. `HDMI_CONSOLE=1` builds
the variant with console output on HDMI. `make clean` between board targets.

The Python helper scripts under `scripts/` are checked with `black`, `isort` and
`flake8`; the shell scripts with `shellcheck`. CI runs all four.

## Reporting problems

Bugs and feature requests belong in the [issue tracker][issues]. Security
findings do not; see [SECURITY.md](SECURITY.md).

The upstream [wiki][wiki] remains the reference documentation for hardware
setup, configuration and the FAQ. It describes the original project and is not
maintained here, so anything specific to this fork is documented in the
[README](README.md) and the [changelog](CHANGELOG.md).

[cc]: https://www.conventionalcommits.org/en/v1.0.0/
[issues]: https://github.com/metaneutrons/mt32-pi/issues
[upstream]: https://github.com/dwhinham/mt32-pi
[wiki]: https://github.com/dwhinham/mt32-pi/wiki
