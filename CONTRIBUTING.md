# Contributing to openXdox

`openXdox` is one project spread across three repositories:

| role | repository | holds |
|---|---|---|
| assembly | [`opensoft/openXdox`](https://github.com/opensoft/openXdox) | this manifest, the pins, the gate |
| spec | [`opensoft/openXdox-spec`](https://github.com/opensoft/openXdox-spec) | requirements, decisions, acceptance |
| code | [`opensoft/openXdox-code`](https://github.com/opensoft/openXdox-code) | the implementation and its tests |

## Where things go

- **Issues** are filed on this repository, `opensoft/openXdox` — even an issue
  that turns out to belong to a leg. Triage moves the conversation, not the
  filing location.
- **Pull requests** go to the leg that owns the change: a requirements or
  acceptance-criteria change is a pull request against `openXdox-spec`; an
  implementation or test change is a pull request against `openXdox-code`. A
  change to the project's own manifest, its pins, or its posture (this file,
  `CODE_OF_CONDUCT.md`, `SECURITY.md`, `LICENSE`, `contracts/`) is a pull
  request against this repository, the assembly root.
- Every repository requires a pull request onto its default branch — nobody
  pushes directly to `main` in any of the three, including this one.

## The lockstep pins move by tool, never by hand

This repository's `contracts/spec-pin.yaml` and `contracts/code-pin.yaml`
each record a leg's commit and tree digest, and the gitlink at that leg's
path names the same commit. `scripts/validate-pins.py` — the `validate`
check that runs on every pull request here — refuses when the gitlink, the
pin file, and any workflow `@<sha>` reference to that leg disagree.

Advancing a leg's pin after a change lands in `openXdox-spec` or
`openXdox-code` is **`scripts/bump-leg.py`**, from a checkout of the
`opensoft/openRepoShape` standard this project was scaffolded from — never a
bare `git submodule update` followed by a hand-edited pin file. That is
exactly the mistake the invariant exists to catch: moving the gitlink alone
and leaving the pin file behind (or vice versa) is drift the next pull
request's `validate` check reports as red, usually on somebody else's
unrelated change.

## License

`openXdox`, and both of its legs, are Apache-2.0 (see `LICENSE`). By
submitting a contribution, you agree it is licensed under the same terms —
inbound is outbound.

## Code of conduct and security reports

Participation in this project is governed by [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).
Security vulnerabilities are reported privately per [SECURITY.md](SECURITY.md),
never as a public issue.
