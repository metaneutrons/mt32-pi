# Security policy

## Supported versions

Only the most recent release is supported. mt32-pi is a bare-metal kernel for
the Raspberry Pi; there are no backported fixes for older releases.

| Version | Supported |
| --- | --- |
| latest release | yes |
| anything older | no |

## Reporting a vulnerability

Report privately through GitHub's [private vulnerability reporting][pvr] on this
repository. Do not open a public issue and do not discuss the finding in a pull
request before it is fixed.

Expect an acknowledgement within seven days and an assessment within thirty
days. If a fix is warranted it ships in the next release, and the advisory is
published once that release is available.

## Scope

This project runs bare metal without an operating system and processes MIDI
input, SoundFont and configuration files from an SD card, and, when networking
is enabled, FTP and mDNS traffic on the local network. Findings in those paths
are in scope, as are findings in the build and release pipeline.

Findings in the upstream projects this one builds on, [Circle][circle],
[Munt][munt] and [FluidSynth][fluidsynth], belong to those projects. Report them
there; a note here is welcome so the submodule pin can be moved once a fix is
released.

[circle]: https://github.com/rsta2/circle
[fluidsynth]: https://github.com/FluidSynth/fluidsynth
[munt]: https://github.com/munt/munt
[pvr]: https://github.com/metaneutrons/mt32-pi/security/advisories/new
