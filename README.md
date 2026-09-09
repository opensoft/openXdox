# openXdox

The ASSEMBLY ROOT of the `openxdox` project — the repository you clone.
It holds no product code of its own: it holds the manifest that says what this
project IS, the two legs as submodules, and the pins that say which commit of
each leg this project is.

Scaffolded from [opensoft/openRepoShape](https://github.com/opensoft/openRepoShape)
at `e9c4827b85f50503bbdd9e5b4fac9d6c3d0baf63`. Elected by Brett Heap on 2026-09-05, against
`openxFactory docs/project-repo-schema.md`.

## Get started

```sh
git clone --recurse-submodules https://github.com/opensoft/openXdox.git
cd openXdox
make bootstrap
```

`make bootstrap` puts each leg on the `main` branch **at its
pinned commit** (so you are not staring at a detached HEAD), runs the three
neutral validators, and prints whatever review authority a wallet register
names for this project — or says plainly that authority is not wallet-carried
here, and continues.

## The three legs

| role | repository | path | holds |
|---|---|---|---|
| assembly | `opensoft/openXdox` | `.` | this manifest, the pins, the gate |
| spec | `opensoft/openXdox-spec` | `spec/` | requirements, decisions, acceptance |
| code | `opensoft/openXdox-code` | `code/` | the implementation and its tests |

All three carry the GitHub topic `xf-project-openxdox`, so the organisation's own search
surfaces the group without a checkout.

## Reading private legs in CI: a GitHub App first, `SHAPE_LEGS_TOKEN` as fallback

If `opensoft/openXdox-spec` or `opensoft/openXdox-code` is **private or internal**,
the `validate` workflow's default `GITHUB_TOKEN` cannot clone it as a
submodule.

Preferred: a dedicated GitHub App — permissions **Contents: Read-only** and
**Metadata: Read**, installed on this organisation with access to the legs —
mints a short-lived token at run time:

```sh
gh secret set SHAPE_LEGS_APP_ID --org <your-org> --body '<app id>'
gh secret set SHAPE_LEGS_APP_PRIVATE_KEY --org <your-org> < app-private-key.pem
```

Fallback: a fine-grained **`SHAPE_LEGS_TOKEN`** PAT, `contents:read` on the
LEGS ONLY:

```sh
gh secret set SHAPE_LEGS_TOKEN --org <your-org> --body '<token>'
```

**On the GitHub Free plan, set these as REPOSITORY secrets.** GitHub delivers
an ORGANISATION secret only to PUBLIC repositories on Free, so on a private
repository `secrets.SHAPE_LEGS_APP_ID` is the empty string — silently — the
App steps skip, and `validate` goes GREEN with the lockstep pin check degraded
away rather than red. Use `--repo <org>/<Repo>` in place of `--org <your-org>`
above, or upgrade the organisation to Team. (Measured on InkRouter,
2026-09-04.)

The root repository itself is always readable by the workflow's own default
token, so `actions/checkout` never carries a `token:` override — putting a
legs-scoped credential there instead is what broke the ROOT checkout with a
403 the first time a PAT was tried for real. Whichever credential resolves is
read only inside the guarded "fetch the legs (submodules)" step, scoped to
that step's `env:`, and used through a `git -c url.<...>.insteadOf=<...>`
rewrite covering both HTTPS and SSH leg URLs — it never touches the root
checkout.

`validate` tries the App first — a `mint a leg-reader token from the GitHub
App` step, scoped by `repositories:` to the legs this organisation itself
owns (a leg under a different owner is excluded with a warning, since an
installation token is per-owner) — and falls back to `SHAPE_LEGS_TOKEN` when
the App is not configured. A configured App that fails to mint fails the job
outright, naming both secrets and the required installation, rather than
degrading.

Without either credential the workflow does not go red on that account: it
checks out the root without submodules, tries `git submodule update --init
--recursive` best-effort, and — if that fails — still runs the naming and
manifest checks, skips `validate-pins.py` with a warning explaining why, and
only fails outright if a credential (App or PAT) **is** configured and the
fetch still failed — naming which source it used. That presence check reads
job-level `env: SHAPE_LEGS_APP_SET` / `SHAPE_LEGS_TOKEN_SET` booleans rather
than the `secrets` context directly in the step's `if:` — the `secrets`
context is not allowed in a step-level `if:` expression, and using it there
makes GitHub reject the whole workflow file instead of just that step.

## The lockstep invariant

For each leg, THREE things name the same commit and they move in ONE commit:

1. the **gitlink** — the `160000` entry recorded at the leg's path
2. **`commit:`** in `contracts/<role>-pin.yaml`
3. every **`.github/workflows/*.yml` `@<sha>`** reference naming that leg

`python3 scripts/validate-pins.py` (also `make pins`, also the `validate` check
on every pull request) refuses if they disagree, and recomputes the leg's tree
digest on top. Advancing a pin is therefore one commit that touches the
submodule, the pin file, and any workflow ref — never a bare `git submodule
update` followed by a commit.

This is written down because the family learned it the expensive way: seven
consecutive pin-syncs in the xFactory aggregation moved the gitlink alone and
left `validate` red on every pull request for a day, unnoticed because the
check runs on pull requests only.

## What the election confers

Nothing. Electing this shape changes no gate, no floor, no grant and no
clearance eligibility; a one-repository project is reviewed identically,
because the authority travels in the grants rather than in the layout. The
`role:` fields in `project.yaml` are navigation. A tool that reads `role: spec`
as "spec authority lives here" has quietly turned a layout into a governance
boundary, and is defective.

## Posture

This project is public from day one: [CONTRIBUTING.md](CONTRIBUTING.md) says
where issues and pull requests go across the three repositories and how the
lockstep pins move; [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) is the
Contributor Covenant v2.1; [SECURITY.md](SECURITY.md) is the private
vulnerability-reporting path; [LICENSE](LICENSE) is Apache-2.0. Both legs
carry their own `LICENSE` and `SECURITY.md` and point back at this
repository's `CONTRIBUTING.md`/`CODE_OF_CONDUCT.md`. In all three, the
`validate` check is a required status check on `main`, enforced by a
repository ruleset — see [docs/branch-protection.md](docs/branch-protection.md).

## Layout

```
AGENTS-shape.md                  the RULES OF THE SHAPE, for an agent (copied)
AGENTS.md                        this project's own instructions (yours)
CLAUDE.md                        one line, pointing at AGENTS.md
project.yaml                     the manifest — the SOURCE of this group
contracts/repository-naming.yaml the four naming families (copied from the shape)
contracts/spec-pin.yaml          the spec leg's commit + tree digest
contracts/code-pin.yaml          the code leg's commit + tree digest
contracts/shape-pin.yaml         the openRepoShape revision + per-file digests
scripts/bootstrap.py             the one command after a recursive clone
scripts/validate-manifest.py     project.yaml, and the legs' names
scripts/validate-pins.py         THE LOCKSTEP VALIDATOR
scripts/validate-repository-naming.py
scripts/repo_shape.py            shared helpers, standard library only
.github/workflows/validate.yml   the neutral gate, on pull_request
```

Everything under `scripts/`, plus `contracts/repository-naming.yaml` and
`AGENTS-shape.md`, is a COPY from `opensoft/openRepoShape`, digest-pinned in
`contracts/shape-pin.yaml`. Edit them upstream, not here — a local edit is
reported as drift. `AGENTS.md` and `CLAUDE.md` have no row and are this
project's own.

## Documentation

The doc index for this repository. Everything under `docs/` is listed here,
and a new document is linked from this table in the same pull request that
adds it — the xFactory family's standing rule, levelled across all six
`opendox`/`openxdox` repositories by the OQ-O scaffold pass
(`opensoft/openxFactory#656`).

| document | what it is |
|---|---|
| [docs/branch-protection.md](docs/branch-protection.md) | the repository ruleset that makes `validate` a required status check on `main`, its `evaluate` → `active` history, and the one policy difference between the two families |
