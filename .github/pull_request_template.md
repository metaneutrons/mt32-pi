<!--
The title becomes the subject line on main after the squash merge and decides
the next version and the changelog rubric. It must follow Conventional Commits,
for example "fix(lcd): wake the display for system messages".

One pull request carries changes of one kind. If this one mixes a feature with
a fix, split it.
-->

## What changes

_A short description of the change and why it is needed._

## How it was verified

_Which board targets were built, on which hardware it was tested, and which
checks were run. State plainly what was not verified._

- [ ] `make -j BOARD=pi3-64` succeeds
- [ ] tested on real hardware, or explicitly not tested

## Notes for the reviewer

_Anything worth knowing that is not obvious from the diff._
